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
این property یه لیستی از کاراکتر میگیره و برای هر فیلد چک میکنه، اگه فیلدی در پیشوندش کاراکترهای داخل این property رو داشته باشه ، اون بخش از فیلد روی متد getter و setter مربوطه ش ignore میشه . مثال :
```
@Getter @Setter
@Accessors(prefix = {"user", "hash$"})
public class LombokAccessorsDemo3 {

  private String userName;
  private String hash$key;  
}
```

```
public class LombokAccessorsDemo3 {

  private String userName;
  private String hash$key;

  public String getName() {
    return this.userName;
  }
  public String getKey() {
    return this.hash$key;
  }
  public void setName(final String userName) {
    this.userName = userName;
  }

  public void setKey(final String hash$key) {
    this.hash$key = hash$key;
  }
}
```

## @SerializedName
برای درک این انوتیشن ابتدا یه توضیح کوتاه راجع به serialization میدم، serialization برای تبدیل یه object به یه json string هست و بالعکس، deserialization برای تبدیل json string به object هستش. برای مثال :
```
public class User{
    private String userName;
    private Integer userAge;

    public User(String name, Integer age){
        userName = name;
        userAge = age;
    }
}
```
در ادامه object user رو serialize میکنیم:
```
User user = new User("Ahmed", 30);
Gson gson = new Gson();
String jsonString = gson.toJson(user);
```
خروجی مدنظر :
```
{
    "userName":"Ahmed",
    "userAge":30
}
```
حالا اگه من انوتیشن SerializedName رو توی object user استفاده کنم، به شکل زیر میشه :
```
public class User{

    @SerializedName("name")
    private String userName;
    @SerializedName("age")
    private Integer userAge;

    public User(String name, Integer age){
        userName = name;
        userAge = age;
    }
}
```
خروجی هم مثل قبل :
```
{
    "name":"Ahmed",
    "age":30
}
```

## @Expose
این انوتیشن اجازه کنترل serialize و deserialize رو به ما میده و برای این کار دو پارامتر serialize و deserialize داره که به صورت default هر جفتشون true هست. به عنوان مثال برای serialize و deserialize با استفاده از Expose@ ابتدا یه gson object میسازیم:
```
Gson gsonBuilder = new GsonBuilder().excludeFieldsWithoutExposeAnnotation().create();
```
حالا طبق قطعه کد زیر برای userName ، پارامتر `deserialize=false` میکنیم در نتیجه این فیلد مقدار `null` رو به ما برمیگردونه:
```
@SerializedName("name")
@Expose(deserialize = false)
private String userName;
```
به همین شکل اگه برای userName پارامتر `serialize=false` باشه :
```
@SerializedName("name")
@Expose(serialize = false)
private String userName;
```
در نتیجه json string خروجی فقظ userAge رو نشون میده :
```
{
    "age":30
}
```

## @Inject
این انوتیشن مشابه @Authwire در spring  عملکرد کاملا مشابه با اون رو داره ، و جفت شون بحث dependency injection یا به اصطلاح wiring کردن object ها رو در سطح اپلیکیشن انجام میدن
نکته : اسپرینگ توصیه کرده به جای @Authwire بهتره با final کردن وابستگی ها و هم چنین گذاشتن انوتیشن @RequiredArgsConstructor این کار انجام بشه ، چون در بهینه کردن استفاده از resource ها ( مموری) تاثیر گذاره ( استفاده از @Authwire و @Inject هر وابستگی ای در سطح اپلیکیشن وجود داشته باشه رو اپلیکیشن ما در compile time میسازه و بعد در run time بلافاصله بعد از compile اونا رو توی context نگهداری میکنه اما با استفاده از @RequiredArgsconstructor فقط در runtime هر موقع که نیاز به استفاده از یه dependency وجود داشته باشه از اون ایجاد میکنه)) 
