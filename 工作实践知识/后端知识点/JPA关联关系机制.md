# JPA关联关系机制

### JPA配置详情

```java
public class BillDetail {
    @ManyToOne
    @JoinColumn(name = "bill_id")
    private Bill bill;
}  

public class Bill {
    @OneToMany(cascade = {CascadeType.PERSIST, CascadeType.MERGE, CascadeType.REMOVE}, mappedBy = "bill")
    @JsonProperty(value = "bill_details")
    private List<BillDetail> billDetails;
}
```

上述代码中，有一个`JoinColumn`注解，这个注解表示该实体(`BillDetail`)为这段关系的**维护方**，`mappedBy`这个注解表示该实体(`Bill`)为这段关系的被**维护方**。(`@ManyToMany`同理，不过是`JoinColumn`变为`JoinTable`)

**关键规则**：只有**关系维护方**才能设置外键。

### 案例

```java
Bill bill = new Bill();
bill.setBillDetails(billDetails); // bill设置了关联的 billDetails

// 但如果 billDetails 中的每个 Bill 没有设置：
// billDetails.setBill(bill);

repository.save(bill); // 保存
```

**结果**：

* `Bill` 表会正常插入记录
* `BillDetail` 表会插入记录，但 `bill_id` 字段为 **NULL**

所以根据上述，一般在`Bill`实体中的`setBillDetails`方法需要设置成下面这种

```java
public void setBillDetails(List<BillDetail> billDetails) {
    billDetails.forEach(billDetail -> billDetail.setBill(this));
    this.billDetails = billDetails;
}
```
