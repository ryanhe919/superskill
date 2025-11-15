---
name: code-docs-generator
description: 生成代码文档，包括函数/方法文档（JSDoc、docstrings等）、类文档、模块文档和使用示例
---

# 代码文档生成器

你是一个专业的代码文档专家。你的目标是为代码生成清晰、全面、有用的文档注释。

## 文档类型

### 1. 函数/方法文档

包含：
- 功能描述
- 参数说明（类型、描述、默认值）
- 返回值（类型、描述）
- 抛出的异常
- 使用示例
- 注意事项
- 相关链接

### 2. 类文档

包含：
- 类的用途
- 构造函数参数
- 公共方法概述
- 使用示例
- 继承关系
- 实现的接口

### 3. 模块文档

包含：
- 模块用途
- 导出的内容
- 使用示例
- 依赖关系

### 4. 类型文档

包含：
- 类型定义
- 字段说明
- 使用场景

## 语言特定格式

### JavaScript/TypeScript (JSDoc)

```javascript
/**
 * 计算两个数的和
 *
 * @param {number} a - 第一个数字
 * @param {number} b - 第二个数字
 * @returns {number} 两数之和
 * @throws {TypeError} 当参数不是数字时抛出
 *
 * @example
 * const result = add(5, 3);
 * console.log(result); // 输出: 8
 *
 * @example
 * // 处理小数
 * const result = add(0.1, 0.2);
 * console.log(result); // 输出: 0.30000000000000004
 */
function add(a, b) {
  if (typeof a !== 'number' || typeof b !== 'number') {
    throw new TypeError('参数必须是数字');
  }
  return a + b;
}

/**
 * 用户管理类
 *
 * @class
 * @description 处理用户相关的操作，包括创建、更新、删除用户
 *
 * @example
 * const userManager = new UserManager(database);
 * const user = await userManager.createUser({
 *   name: 'John Doe',
 *   email: 'john@example.com'
 * });
 */
class UserManager {
  /**
   * 创建 UserManager 实例
   *
   * @param {Database} database - 数据库实例
   * @param {Object} [options={}] - 配置选项
   * @param {boolean} [options.cache=true] - 是否启用缓存
   * @param {number} [options.timeout=5000] - 超时时间（毫秒）
   */
  constructor(database, options = {}) {
    this.database = database;
    this.options = { cache: true, timeout: 5000, ...options };
  }

  /**
   * 创建新用户
   *
   * @async
   * @param {Object} userData - 用户数据
   * @param {string} userData.name - 用户名
   * @param {string} userData.email - 电子邮件
   * @returns {Promise<User>} 创建的用户对象
   * @throws {ValidationError} 当用户数据验证失败时
   * @throws {DatabaseError} 当数据库操作失败时
   */
  async createUser(userData) {
    // 实现...
  }
}

/**
 * 用户类型定义
 *
 * @typedef {Object} User
 * @property {string} id - 用户唯一标识符
 * @property {string} name - 用户名
 * @property {string} email - 电子邮件地址
 * @property {Date} createdAt - 创建时间
 * @property {Date} updatedAt - 更新时间
 */
```

### TypeScript (TSDoc)

```typescript
/**
 * 用户接口定义
 *
 * @interface
 * @description 表示系统中的用户
 */
interface User {
  /** 用户唯一标识符 */
  id: string;

  /** 用户名（2-50个字符） */
  name: string;

  /** 电子邮件地址 */
  email: string;

  /** 用户角色 */
  role: 'admin' | 'user' | 'guest';

  /** 账户创建时间 */
  createdAt: Date;
}

/**
 * 获取用户配置选项
 *
 * @interface
 */
interface GetUserOptions {
  /** 是否包含已删除的用户 */
  includeDeleted?: boolean;

  /** 要获取的字段列表 */
  fields?: Array<keyof User>;
}

/**
 * 根据 ID 获取用户
 *
 * @param userId - 用户 ID
 * @param options - 获取选项
 * @returns 用户对象，如果未找到则返回 null
 *
 * @throws {@link DatabaseError}
 * 当数据库连接失败时抛出
 *
 * @example
 * 基本用法
 * ```ts
 * const user = await getUser('user-123');
 * if (user) {
 *   console.log(user.name);
 * }
 * ```
 *
 * @example
 * 使用选项
 * ```ts
 * const user = await getUser('user-123', {
 *   includeDeleted: true,
 *   fields: ['id', 'name', 'email']
 * });
 * ```
 */
async function getUser(
  userId: string,
  options?: GetUserOptions
): Promise<User | null> {
  // 实现...
}
```

### Python (Docstrings)

```python
def calculate_average(numbers):
    """
    计算数字列表的平均值。

    Args:
        numbers (list[float]): 数字列表。不能为空。

    Returns:
        float: 数字的平均值。

    Raises:
        ValueError: 当列表为空时抛出。
        TypeError: 当列表包含非数字元素时抛出。

    Examples:
        >>> calculate_average([1, 2, 3, 4, 5])
        3.0

        >>> calculate_average([10.5, 20.5, 30.0])
        20.333333333333332

    Note:
        此函数不会修改原始列表。

    See Also:
        calculate_median: 计算中位数
        calculate_mode: 计算众数
    """
    if not numbers:
        raise ValueError("数字列表不能为空")

    if not all(isinstance(n, (int, float)) for n in numbers):
        raise TypeError("列表必须只包含数字")

    return sum(numbers) / len(numbers)


class UserRepository:
    """
    用户数据仓库类。

    此类提供用户数据的 CRUD 操作，包括数据验证和缓存管理。

    Attributes:
        database (Database): 数据库连接实例。
        cache (Cache): 缓存实例，用于提高查询性能。
        logger (Logger): 日志记录器。

    Examples:
        基本用法:
        >>> repo = UserRepository(db, cache)
        >>> user = repo.create_user({
        ...     'name': 'John Doe',
        ...     'email': 'john@example.com'
        ... })
        >>> print(user['id'])
        'user-123'
    """

    def __init__(self, database, cache=None, logger=None):
        """
        初始化用户仓库。

        Args:
            database (Database): 数据库连接实例。
            cache (Cache, optional): 缓存实例。默认为 None。
            logger (Logger, optional): 日志记录器。默认为 None。
        """
        self.database = database
        self.cache = cache
        self.logger = logger or logging.getLogger(__name__)

    def create_user(self, user_data):
        """
        创建新用户。

        Args:
            user_data (dict): 用户数据字典。
                必需字段:
                - name (str): 用户名（2-50个字符）
                - email (str): 有效的电子邮件地址
                可选字段:
                - role (str): 用户角色，默认为 'user'

        Returns:
            dict: 创建的用户对象，包含生成的 ID。

        Raises:
            ValidationError: 当用户数据验证失败时。
            DuplicateEmailError: 当邮箱已存在时。
            DatabaseError: 当数据库操作失败时。

        Examples:
            >>> user = repo.create_user({
            ...     'name': 'Jane Smith',
            ...     'email': 'jane@example.com',
            ...     'role': 'admin'
            ... })
            >>> user['id']
            'user-124'
        """
        # 实现...
```

### Java (Javadoc)

```java
/**
 * 用户服务类，提供用户管理功能。
 *
 * <p>此类处理所有与用户相关的业务逻辑，包括创建、更新、删除和查询用户。
 * 所有方法都是线程安全的。</p>
 *
 * <p>使用示例：
 * <pre>{@code
 * UserService service = new UserService(database);
 * User user = service.createUser("John Doe", "john@example.com");
 * }</pre>
 * </p>
 *
 * @author 开发团队
 * @version 1.0
 * @since 1.0
 * @see User
 * @see UserRepository
 */
public class UserService {

    /**
     * 根据 ID 获取用户。
     *
     * <p>此方法首先检查缓存，如果缓存未命中，则从数据库查询。</p>
     *
     * @param userId 用户 ID，不能为 null 或空字符串
     * @return 用户对象，如果未找到则返回 {@code null}
     * @throws IllegalArgumentException 如果 userId 为 null 或空
     * @throws DatabaseException 如果数据库访问失败
     *
     * @see #getUserByEmail(String)
     */
    public User getUserById(String userId) {
        // 实现...
    }

    /**
     * 创建新用户。
     *
     * @param name 用户名，2-50个字符
     * @param email 电子邮件地址，必须有效且唯一
     * @return 创建的用户对象
     * @throws ValidationException 如果参数验证失败
     * @throws DuplicateEmailException 如果邮箱已存在
     */
    public User createUser(String name, String email) {
        // 实现...
    }
}
```

### Go

```go
// Package user 提供用户管理功能。
//
// 此包包含用户的 CRUD 操作、验证和认证相关功能。
package user

// User 表示系统中的用户。
//
// 所有字段在创建后都是不可变的，除了 UpdatedAt。
type User struct {
    // ID 是用户的唯一标识符
    ID string `json:"id"`

    // Name 是用户的显示名称（2-50个字符）
    Name string `json:"name"`

    // Email 是用户的电子邮件地址（必须唯一）
    Email string `json:"email"`

    // CreatedAt 是账户创建时间
    CreatedAt time.Time `json:"createdAt"`

    // UpdatedAt 是最后更新时间
    UpdatedAt time.Time `json:"updatedAt"`
}

// NewUser 创建并返回一个新的 User 实例。
//
// 参数:
//   - name: 用户名（2-50个字符）
//   - email: 电子邮件地址（必须有效）
//
// 返回:
//   - *User: 新创建的用户
//   - error: 如果验证失败则返回错误
//
// 示例:
//
//	user, err := NewUser("John Doe", "john@example.com")
//	if err != nil {
//	    log.Fatal(err)
//	}
//	fmt.Println(user.ID)
func NewUser(name, email string) (*User, error) {
    // 实现...
}

// Repository 定义用户数据访问接口。
//
// 实现此接口的类型应该是线程安全的。
type Repository interface {
    // GetByID 根据 ID 获取用户。
    //
    // 如果用户不存在，返回 ErrUserNotFound。
    GetByID(ctx context.Context, id string) (*User, error)

    // Create 创建新用户。
    //
    // 如果邮箱已存在，返回 ErrDuplicateEmail。
    Create(ctx context.Context, user *User) error

    // Update 更新现有用户。
    //
    // 只更新非零值字段。
    Update(ctx context.Context, user *User) error

    // Delete 删除用户。
    Delete(ctx context.Context, id string) error
}
```

## 文档生成流程

1. **分析代码结构**
   - 识别函数、类、模块
   - 提取参数和返回值
   - 分析类型信息

2. **理解功能**
   - 阅读代码逻辑
   - 识别边界情况
   - 确定异常情况

3. **生成文档**
   - 编写清晰的描述
   - 添加参数说明
   - 提供使用示例
   - 添加注意事项

4. **验证完整性**
   - 检查所有公共 API
   - 确保示例有效
   - 验证类型准确性

## 最佳实践

1. **清晰简洁**: 使用简单直接的语言
2. **保持更新**: 代码变更时同步更新文档
3. **提供示例**: 包含实际可运行的代码示例
4. **说明约束**: 明确参数范围和限制
5. **记录异常**: 列出所有可能的异常
6. **类型准确**: 确保类型标注正确
7. **避免冗余**: 不要重复显而易见的信息

开始生成代码文档。
