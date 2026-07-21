# Android开发架构指南

## Android Framework核心组件

### 1. 四大组件

#### Activity
- **职责**：用户界面的展示和用户交互
- **生命周期**：onCreate() → onStart() → onResume() → onPause() → onStop() → onDestroy()
- **最佳实践**：
  - 避免在Activity中处理复杂的业务逻辑
  - 合理管理资源，及时释放
  - 正确处理生命周期回调

#### Service
- **职责**：在后台执行长时间运行的操作
- **类型**：
  - Started Service：通过startService()启动
  - Bound Service：通过bindService()绑定
- **最佳实践**：
  - 考虑使用IntentService处理异步任务
  - Android O及以上版本注意后台限制

#### BroadcastReceiver
- **职责**：接收和响应系统或其他应用发出的广播消息
- **类型**：
  - 动态注册：运行时注册，应用退出后失效
  - 静态注册：在AndroidManifest.xml中声明
- **最佳实践**：
  - Android O及以上版本静态广播受限
  - 及时注销动态广播接收器

#### ContentProvider
- **职责**：在不同应用之间共享数据
- **最佳实践**：
  - 使用SQLiteOpenHelper管理数据库
  - 注意数据安全和权限控制

### 2. Fragment
- **职责**：Activity界面的一部分，可复用的UI组件
- **生命周期**：与Activity生命周期紧密相关
- **最佳实践**：
  - 使用Fragment替代Activity以提高性能
  - 合理管理Fragment事务

## Android应用架构最佳实践

### 1. 分层架构设计

#### 数据层 (Data Layer)
- 负责数据的获取和存储
- 包括本地数据库、网络请求、文件操作等
- 使用Repository模式统一数据访问接口

#### 领域层 (Domain Layer)
- 包含业务逻辑和Use Case
- 独立于Android Framework
- 易于测试和复用

#### 表示层 (Presentation Layer)
- 负责UI展示和用户交互
- 在Android中通常对应Activity/Fragment + Presenter/ViewModel

### 2. 数据持久化

#### Room数据库
```java
@Entity(tableName = "users")
public class User {
    @PrimaryKey
    public int id;
    
    public String name;
    
    public String email;
}

@Dao
public interface UserDao {
    @Query("SELECT * FROM users")
    List<User> getAllUsers();
    
    @Insert
    void insertUser(User user);
    
    @Update
    void updateUser(User user);
    
    @Delete
    void deleteUser(User user);
}

@Database(entities = {User.class}, version = 1)
public abstract class AppDatabase extends RoomDatabase {
    public abstract UserDao userDao();
}
```

#### SharedPreferences
- 适用于简单的键值对存储
- 注意不要存储敏感信息
- 考虑使用加密存储敏感数据

### 3. 网络请求

#### Retrofit + OkHttp
```java
public interface ApiService {
    @GET("users/{id}")
    Call<User> getUser(@Path("id") int userId);
    
    @POST("users")
    Call<User> createUser(@Body User user);
}

// 创建Retrofit实例
Retrofit retrofit = new Retrofit.Builder()
    .baseUrl("https://api.example.com/")
    .addConverterFactory(GsonConverterFactory.create())
    .build();

ApiService service = retrofit.create(ApiService.class);
```

### 4. 异步处理

#### AsyncTask (已废弃)
- Android 11开始标记为废弃
- 不推荐在新项目中使用

#### Handler + Thread
```java
Handler mainHandler = new Handler(Looper.getMainLooper());
new Thread(() -> {
    // 执行耗时操作
    String result = doBackgroundWork();
    
    // 切换到主线程更新UI
    mainHandler.post(() -> {
        updateUI(result);
    });
}).start();
```

#### ExecutorService
```java
ExecutorService executor = Executors.newFixedThreadPool(4);
executor.execute(() -> {
    // 后台任务
});

// 记得在适当时机关闭
executor.shutdown();
```

#### RxJava
- 提供强大的异步编程能力
- 支持线程切换、背压处理等高级特性

## 性能优化建议

### 1. 内存优化

#### 避免内存泄漏
```java
// 使用静态内部类避免持有外部类引用
public static class ViewHolder {
    TextView textView;
}

// 及时释放资源
@Override
protected void onDestroy() {
    super.onDestroy();
    if (subscription != null && !subscription.isUnsubscribed()) {
        subscription.unsubscribe();
    }
}
```

#### 合理使用Bitmap
```java
// 及时回收Bitmap
if (bitmap != null && !bitmap.isRecycled()) {
    bitmap.recycle();
    bitmap = null;
}

// 使用合适的图片尺寸
BitmapFactory.Options options = new BitmapFactory.Options();
options.inJustDecodeBounds = true;
BitmapFactory.decodeResource(getResources(), R.drawable.image, options);
```

### 2. UI优化

#### 减少布局嵌套
- 使用ConstraintLayout替代复杂的RelativeLayout和LinearLayout嵌套
- 合理使用merge标签减少视图层级

#### 优化列表性能
- 使用ViewHolder模式
- 合理设置RecyclerView的缓存策略
- 避免在getView()中执行耗时操作

### 3. 启动优化

#### 冷启动优化
- 减少Application.onCreate()中的耗时操作
- 延迟初始化非必要组件
- 使用启动页提升用户体验

## 架构整改常见问题及解决方案

### 1. Activity过重问题

**问题表现**：
- Activity中包含大量业务逻辑代码
- Activity承担了不属于它的职责

**解决方案**：
- 引入MVP/MVVM架构模式
- 将业务逻辑下沉到Presenter/ViewModel层
- 使用Use Case封装领域逻辑

### 2. 网络请求管理混乱

**问题表现**：
- 网络请求分散在各个Activity中
- 缺乏统一的错误处理机制
- 没有合理的缓存策略

**解决方案**：
- 封装网络请求库，提供统一入口
- 实现拦截器统一处理认证、日志等
- 引入Repository模式统一数据源

### 3. 数据存储不规范

**问题表现**：
- SharedPreferences滥用
- 数据库存储结构不合理
- 缺乏数据迁移方案

**解决方案**：
- 合理选择存储方式
- 使用Room等ORM框架管理数据库
- 实现数据库升级策略

### 4. 异步任务管理不当

**问题表现**：
- 异步任务生命周期管理混乱
- 缺乏任务取消机制
- 主线程阻塞问题

**解决方案**：
- 使用现代异步框架(RxJava、LiveData等)
- 实现任务生命周期感知
- 合理安排线程池大小

## Android架构组件推荐

### 1. ViewModel
- 负责准备和管理UI相关数据
- 感知生命周期，避免内存泄漏
- 在配置变更时保留数据

### 2. LiveData
- 可观察的数据持有者
- 遵循观察者模式
- 自动处理生命周期

### 3. Room
- SQLite对象映射库
- 编译时验证SQL查询
- 与LiveData集成良好

### 4. Paging
- 分页加载数据
- 内置加载状态管理
- 与RecyclerView无缝集成

## 总结

良好的Android应用架构应该是：
1. **清晰的分层**：各层职责明确，依赖关系合理
2. **高内聚低耦合**：模块内部高度相关，模块间依赖最小
3. **易测试**：核心业务逻辑不依赖Android Framework，便于单元测试
4. **可扩展**：能够方便地添加新功能而不影响现有代码
5. **高性能**：合理使用资源，避免内存泄漏和性能瓶颈

在进行架构整改时，应该根据项目实际情况逐步推进，避免一次性大规模重构带来的风险。