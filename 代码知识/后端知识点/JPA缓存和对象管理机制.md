# JPA缓存和对象管理机制

### 1. 同一会话中的对象唯一性

在同一个持久化上下文（Persistence Context）中，JPA 保证：

* **相同 ID 的实体对象只会有一个实例**
* 所有引用这个 ID 的地方都会指向**同一个对象引用**

### 2. 具体工作原理

当执行 `billRepository.findAll(billIds)` 时：

```java
// JPA 内部处理逻辑大致如下：
Map<Object, Object> entityMap = new HashMap<>(); // 一级缓存

for (Bill bill : queryResult) {
    Hospital hospital = bill.getHospital();
    Object hospitalId = hospital.getId();
  
    if (entityMap.containsKey(hospitalId)) {
        // 如果缓存中已有相同ID的Hospital，使用缓存中的实例
        bill.setHospital((Hospital) entityMap.get(hospitalId));
    } else {
        // 否则将新实例加入缓存
        entityMap.put(hospitalId, hospital);
    }
}
```

### 3. 验证示例

假设数据库中有：

* `Bill1` → `HospitalA` (ID=1)
* `Bill2` → `HospitalA` (ID=1)
* `Bill3` → `HospitalB` (ID=2)

查询后的内存状态：

```java
List<Bill> databaseBills = billRepository.findAll(Arrays.asList(1, 2, 3));

// 这些比较都会返回 true：
databaseBills.get(0).getHospital() == databaseBills.get(1).getHospital(); // true
databaseBills.get(0).getHospital().equals(databaseBills.get(1).getHospital()); // true

// 这个比较返回 false：
databaseBills.get(0).getHospital() == databaseBills.get(2).getHospital(); // false
```
