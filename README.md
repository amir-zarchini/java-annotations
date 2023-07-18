# java-annotations
## @Accessor 
این انوتیشن برای کانفیگ کردن @Getter و @Setter استفاده میشه که چهار property مختص به خودش رو داره:
### fluent
این property این امکان رو میده که کلمه های get و set رو از روی متدهامون برداریم و متدهای get و set رو برای هر فیلد فقط با اسم همون فیلد ساخته بشه. مثال:
```
@Getter @Setter
@Accessors(fluent = true)
public class LombokAccessorsDemo1 {

  private Long id;
  private String name;
}
```

```
public class LombokAccessorsDemo1 {

  private Long id;
  private String name;

  public Long id() {
    return this.id;
  }
  public String name() {
    return this.name;
  }
  public LombokAccessorsDemo1 id(final Long id) {
    this.id = id;
    return this;
  }
  public LombokAccessorsDemo1 name(final String name) {
    this.name = name;
    return this;
  }
}
```

### chain
این property از جنس boolean هستش، اگه `chain=true` باشه این امکان رو میده که متد setter مقدار this رو به جای void برگردونه، بطور default مقدارش false هست، ولی اگر `fluent=true` باشه `chain=true`  . مثال:
```
@Getter @Setter
@Accessors(chain = true)
public class LombokAccessorsDemo2 {

  private Long id;
  private String name;
}
```
```
public class LombokAccessorsDemo2 {
  
  private Long id;
  private String name;

  public Long getId() {
    return this.id;
  }

  public String getName() {
    return this.name;
  }

  public LombokAccessorsDemo2 setId(final Long id) {
    this.id = id;
    return this;
  }

  public LombokAccessorsDemo2 setName(final String name) {
    this.name = name;
    return this;
  }

}
```

### prefix


