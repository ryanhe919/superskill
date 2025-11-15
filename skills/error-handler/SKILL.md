---
name: error-handler
description: 实现健壮的错误处理，包括自定义错误类、Try-Catch模式、全局错误处理器、输入验证、重试逻辑和结构化日志
---

# 错误处理实现

你是一个专业的错误处理专家。你的目标是实现健壮、可维护的错误处理机制。

## 错误处理原则

1. **快速失败**: 尽早发现和报告错误
2. **明确错误**: 提供清晰的错误消息
3. **分层处理**: 在适当的层级处理错误
4. **记录日志**: 记录错误上下文
5. **优雅降级**: 在错误时提供备选方案
6. **用户友好**: 向用户显示有用的错误信息

## 自定义错误类

### JavaScript/TypeScript

```typescript
// 基础错误类
class AppError extends Error {
  constructor(
    message: string,
    public statusCode: number = 500,
    public code: string = 'INTERNAL_ERROR',
    public isOperational: boolean = true
  ) {
    super(message);
    this.name = this.constructor.name;
    Error.captureStackTrace(this, this.constructor);
  }
}

// 特定错误类型
class ValidationError extends AppError {
  constructor(
    message: string,
    public field?: string,
    public value?: any
  ) {
    super(message, 400, 'VALIDATION_ERROR');
    this.name = 'ValidationError';
  }
}

class NotFoundError extends AppError {
  constructor(resource: string, id?: string) {
    const message = id
      ? `${resource} with id '${id}' not found`
      : `${resource} not found`;
    super(message, 404, 'NOT_FOUND');
    this.name = 'NotFoundError';
  }
}

class AuthenticationError extends AppError {
  constructor(message: string = 'Authentication failed') {
    super(message, 401, 'AUTHENTICATION_ERROR');
    this.name = 'AuthenticationError';
  }
}

class AuthorizationError extends AppError {
  constructor(message: string = 'Insufficient permissions') {
    super(message, 403, 'AUTHORIZATION_ERROR');
    this.name = 'AuthorizationError';
  }
}

class DatabaseError extends AppError {
  constructor(
    message: string,
    public query?: string,
    public originalError?: Error
  ) {
    super(message, 500, 'DATABASE_ERROR', false);
    this.name = 'DatabaseError';
  }
}

class ExternalServiceError extends AppError {
  constructor(
    service: string,
    message: string,
    public originalError?: Error
  ) {
    super(`${service}: ${message}`, 502, 'EXTERNAL_SERVICE_ERROR', false);
    this.name = 'ExternalServiceError';
  }
}
```

### Python

```python
class AppError(Exception):
    """应用基础错误类"""

    def __init__(
        self,
        message: str,
        status_code: int = 500,
        code: str = 'INTERNAL_ERROR',
        details: dict = None
    ):
        super().__init__(message)
        self.message = message
        self.status_code = status_code
        self.code = code
        self.details = details or {}

class ValidationError(AppError):
    """验证错误"""

    def __init__(self, message: str, field: str = None, value: any = None):
        super().__init__(
            message=message,
            status_code=400,
            code='VALIDATION_ERROR',
            details={'field': field, 'value': value}
        )

class NotFoundError(AppError):
    """资源未找到错误"""

    def __init__(self, resource: str, resource_id: str = None):
        message = f"{resource}"
        if resource_id:
            message += f" with id '{resource_id}'"
        message += " not found"
        super().__init__(
            message=message,
            status_code=404,
            code='NOT_FOUND'
        )

class DatabaseError(AppError):
    """数据库错误"""

    def __init__(self, message: str, query: str = None, original_error: Exception = None):
        super().__init__(
            message=message,
            status_code=500,
            code='DATABASE_ERROR',
            details={'query': query, 'original_error': str(original_error)}
        )
```

## Try-Catch 模式

### JavaScript/TypeScript

```typescript
// 基本 try-catch
async function processUser(userId: string) {
  try {
    const user = await userRepository.findById(userId);
    if (!user) {
      throw new NotFoundError('User', userId);
    }

    return await processUserData(user);
  } catch (error) {
    if (error instanceof AppError) {
      // 已知的应用错误，直接抛出
      throw error;
    }

    // 未知错误，包装后抛出
    logger.error('Failed to process user', { userId, error });
    throw new AppError('Failed to process user');
  }
}

// 带资源清理的 try-catch
async function processFile(filePath: string) {
  let fileHandle;

  try {
    fileHandle = await fs.promises.open(filePath, 'r');
    const content = await fileHandle.readFile('utf-8');
    return processContent(content);
  } catch (error) {
    logger.error('Failed to process file', { filePath, error });
    throw new AppError('File processing failed');
  } finally {
    // 确保资源被释放
    if (fileHandle) {
      await fileHandle.close();
    }
  }
}

// 多个 catch 块
async function complexOperation() {
  try {
    await riskyOperation();
  } catch (error) {
    if (error instanceof ValidationError) {
      // 处理验证错误
      return handleValidationError(error);
    } else if (error instanceof DatabaseError) {
      // 处理数据库错误
      return handleDatabaseError(error);
    } else {
      // 处理其他错误
      throw error;
    }
  }
}
```

### Python

```python
# 基本 try-except
async def process_user(user_id: str):
    try:
        user = await user_repository.find_by_id(user_id)
        if not user:
            raise NotFoundError('User', user_id)

        return await process_user_data(user)
    except AppError:
        # 已知的应用错误，直接抛出
        raise
    except Exception as e:
        # 未知错误，记录并包装
        logger.error(f'Failed to process user {user_id}', exc_info=True)
        raise AppError('Failed to process user')

# 带资源清理的 try-except-finally
def process_file(file_path: str):
    file_handle = None
    try:
        file_handle = open(file_path, 'r')
        content = file_handle.read()
        return process_content(content)
    except IOError as e:
        logger.error(f'Failed to read file {file_path}', exc_info=True)
        raise AppError('File processing failed')
    finally:
        if file_handle:
            file_handle.close()

# 使用 with 语句自动管理资源
def process_file_safe(file_path: str):
    try:
        with open(file_path, 'r') as f:
            content = f.read()
        return process_content(content)
    except IOError as e:
        logger.error(f'Failed to read file {file_path}', exc_info=True)
        raise AppError('File processing failed')
```

## 全局错误处理

### Express.js (Node.js)

```typescript
import { Request, Response, NextFunction } from 'express';

// 异步错误包装器
function asyncHandler(fn: Function) {
  return (req: Request, res: Response, next: NextFunction) => {
    Promise.resolve(fn(req, res, next)).catch(next);
  };
}

// 使用示例
app.get('/users/:id', asyncHandler(async (req, res) => {
  const user = await userService.getById(req.params.id);
  res.json(user);
}));

// 404 处理器
app.use((req: Request, res: Response, next: NextFunction) => {
  next(new NotFoundError('Route'));
});

// 全局错误处理器
app.use((
  error: Error,
  req: Request,
  res: Response,
  next: NextFunction
) => {
  // 记录错误
  logger.error('Error occurred', {
    error: error.message,
    stack: error.stack,
    url: req.url,
    method: req.method,
    ip: req.ip,
    userId: req.user?.id
  });

  // 处理特定错误类型
  if (error instanceof AppError) {
    return res.status(error.statusCode).json({
      error: {
        message: error.message,
        code: error.code,
        ...(process.env.NODE_ENV === 'development' && {
          stack: error.stack
        })
      }
    });
  }

  // 处理验证错误 (例如来自 express-validator)
  if (error.name === 'ValidationError') {
    return res.status(400).json({
      error: {
        message: 'Validation failed',
        code: 'VALIDATION_ERROR',
        details: error
      }
    });
  }

  // 未知错误
  res.status(500).json({
    error: {
      message: 'Internal server error',
      code: 'INTERNAL_ERROR',
      ...(process.env.NODE_ENV === 'development' && {
        stack: error.stack
      })
    }
  });
});

// 未捕获的异常处理
process.on('uncaughtException', (error: Error) => {
  logger.error('Uncaught Exception', { error });
  process.exit(1);
});

// 未处理的 Promise 拒绝
process.on('unhandledRejection', (reason: any) => {
  logger.error('Unhandled Rejection', { reason });
  process.exit(1);
});
```

### FastAPI (Python)

```python
from fastapi import FastAPI, Request, status
from fastapi.responses import JSONResponse
from fastapi.exceptions import RequestValidationError

app = FastAPI()

# 自定义错误处理器
@app.exception_handler(AppError)
async def app_error_handler(request: Request, exc: AppError):
    logger.error(
        f"Error occurred: {exc.message}",
        extra={
            "code": exc.code,
            "url": str(request.url),
            "method": request.method,
        }
    )

    return JSONResponse(
        status_code=exc.status_code,
        content={
            "error": {
                "message": exc.message,
                "code": exc.code,
                "details": exc.details
            }
        }
    )

# 验证错误处理
@app.exception_handler(RequestValidationError)
async def validation_error_handler(request: Request, exc: RequestValidationError):
    return JSONResponse(
        status_code=status.HTTP_400_BAD_REQUEST,
        content={
            "error": {
                "message": "Validation failed",
                "code": "VALIDATION_ERROR",
                "details": exc.errors()
            }
        }
    )

# 通用错误处理
@app.exception_handler(Exception)
async def generic_error_handler(request: Request, exc: Exception):
    logger.error(
        f"Unexpected error: {str(exc)}",
        exc_info=True,
        extra={
            "url": str(request.url),
            "method": request.method,
        }
    )

    return JSONResponse(
        status_code=status.HTTP_500_INTERNAL_SERVER_ERROR,
        content={
            "error": {
                "message": "Internal server error",
                "code": "INTERNAL_ERROR"
            }
        }
    )
```

## 输入验证

### TypeScript (使用 Zod)

```typescript
import { z } from 'zod';

// 定义 Schema
const createUserSchema = z.object({
  name: z.string()
    .min(2, '名字至少2个字符')
    .max(50, '名字最多50个字符'),
  email: z.string()
    .email('无效的邮箱地址'),
  age: z.number()
    .int('年龄必须是整数')
    .min(18, '年龄必须至少18岁')
    .max(120, '年龄必须小于120岁')
    .optional(),
  role: z.enum(['admin', 'user', 'guest'])
    .default('user')
});

// 使用验证
async function createUser(data: unknown) {
  try {
    const validatedData = createUserSchema.parse(data);
    return await userRepository.create(validatedData);
  } catch (error) {
    if (error instanceof z.ZodError) {
      throw new ValidationError(
        'Invalid user data',
        undefined,
        error.errors
      );
    }
    throw error;
  }
}
```

### Python (使用 Pydantic)

```python
from pydantic import BaseModel, EmailStr, Field, validator

class CreateUserRequest(BaseModel):
    name: str = Field(..., min_length=2, max_length=50)
    email: EmailStr
    age: int = Field(None, ge=18, le=120)
    role: str = 'user'

    @validator('role')
    def validate_role(cls, v):
        if v not in ['admin', 'user', 'guest']:
            raise ValueError('Invalid role')
        return v

# 使用验证
async def create_user(data: dict):
    try:
        user_data = CreateUserRequest(**data)
        return await user_repository.create(user_data.dict())
    except ValidationError as e:
        raise AppValidationError(
            'Invalid user data',
            details=e.errors()
        )
```

## 重试逻辑

```typescript
// 指数退避重试
async function retryWithBackoff<T>(
  fn: () => Promise<T>,
  maxRetries: number = 3,
  baseDelay: number = 1000,
  maxDelay: number = 10000
): Promise<T> {
  let lastError: Error;

  for (let attempt = 0; attempt <= maxRetries; attempt++) {
    try {
      return await fn();
    } catch (error) {
      lastError = error as Error;

      // 不重试的错误类型
      if (
        error instanceof ValidationError ||
        error instanceof AuthenticationError ||
        error instanceof NotFoundError
      ) {
        throw error;
      }

      if (attempt < maxRetries) {
        const delay = Math.min(
          baseDelay * Math.pow(2, attempt),
          maxDelay
        );

        logger.warn(`Retry attempt ${attempt + 1}/${maxRetries}`, {
          error: error.message,
          delay
        });

        await new Promise(resolve => setTimeout(resolve, delay));
      }
    }
  }

  throw new AppError(
    `Failed after ${maxRetries} retries: ${lastError.message}`,
    500,
    'MAX_RETRIES_EXCEEDED'
  );
}

// 使用示例
const result = await retryWithBackoff(
  () => externalApi.fetchData(),
  3,  // 最多重试3次
  1000  // 初始延迟1秒
);
```

## 结构化日志

```typescript
import winston from 'winston';

// 配置 Winston 日志器
const logger = winston.createLogger({
  level: process.env.LOG_LEVEL || 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.errors({ stack: true }),
    winston.format.json()
  ),
  defaultMeta: {
    service: 'my-app',
    environment: process.env.NODE_ENV
  },
  transports: [
    new winston.transports.File({
      filename: 'error.log',
      level: 'error'
    }),
    new winston.transports.File({
      filename: 'combined.log'
    })
  ]
});

// 开发环境添加控制台输出
if (process.env.NODE_ENV !== 'production') {
  logger.add(new winston.transports.Console({
    format: winston.format.simple()
  }));
}

// 使用示例
logger.info('User created', {
  userId: user.id,
  email: user.email
});

logger.error('Database query failed', {
  query: sql,
  error: error.message,
  stack: error.stack
});
```

开始实现错误处理。
