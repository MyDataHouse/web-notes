# nest 后端开发

## 安装

### 脚手架安装

- 安装 `@nestjs/cli`  创建 nest 项目

  ```shell
  pnpm add -g @nestjs/cli
  nest new project-name
  ```

### 手动安装

- 安装 `@nestjs/common` `@nestjs/core` `"@nestjs/platform-express` `reflect-metadata` `rxjs`

  ```shell
  pnpm i @nestjs/common @nestjs/core @nestjs/platform-express rxjs reflect-metadata
  ```

- 配置打包工具处理转换 ts 并完整支持装饰器功能

## 配置数据

- 安装@nestjs/config 扩展包

  ```shell
  pnpm add @nestjs/config
  ```

- 注入使用方式

  1. env 文件注入
  
     ```typescript
      import { ConfigModule } from '@nestjs/config';
        
        @Module({
          imports: [
           	...
            ConfigModule.forRoot({ //系统会自动加载**.env 里的内容
            	//全局模块
              isGlobal: true,
            }),
          ],
         	...
        })
        //============使用方式
        import { Injectable } from '@nestjs/common';
        import { ConfigService } from '@nestjs/config';
        
        @Injectable()
        export class PrismaService  {
          constructor(config: ConfigService) {
        		console.log(config.get('名称'))
          }
        }
     ```
  
     
  
  2. 多文件配置
  
     ```typescript
     //src/config/app.ts
     export default () => ({
       app: {
         name: 'houdunren.com',
         isDev: process.env.NODE_ENV=='development',
       },
       
     })
     //src/config/database.ts
     export default () => ({
       database: {
         url: 'localhost',
       },
     })
     
     //模块声明
     import { ConfigModule } from '@nestjs/config'
     import { Module } from '@nestjs/common'
     import { AppController } from './app.controller'
     import { AppService } from './app.service'
     import config from './config/app.ts'
     import database from './config/database.ts'
     
     @Module({
       imports: [
         ConfigModule.forRoot({
           isGlobal: true,
           //可以加载多个配置文件
           load: [app,database],
         }),
       ],
       controllers: [AppController],
       providers: [AppService],
     })
     export class AppModule {}
     ```
  
  3. 命名空间，有类型提示
  
     ```typescript
     import { registerAs } from '@nestjs/config'
     
     export default registerAs('database', () => ({
       url: process.env.DATABASE_URL,
     }))
     
     //声明
     import config from './config'
     
     @Module({
       imports: [ConfigModule.forRoot({ load: [config] })],
       controllers: [AppController],
       providers: [AppService],
     })
     export class AppModule {}
     
     //=======使用
     import databaseConfig from 'src/config/database.config'
     import { Inject, Injectable } from '@nestjs/common'
     
     @Injectable()
     export class AppService {
       constructor(
         @Inject(databaseConfig.KEY) //注入配置项目
         private config: ReturnType<typeof databaseConfig>,
       ) {}
       getHello(): string {
         return this.config.host
       }
     }
     ```
  
     

## 生命周期

nest.js 请求生命周期大致如下：

1. 收到请求
2. 全局绑定的中间件
3. 模块绑定的中间件
4. 全局守卫
5. 控制层守卫
6. 路由守卫
7. 全局拦截器（控制器之前）
8. 控制器层拦截器 （控制器之前）
9. 路由拦截器 （控制器之前）
10. 全局管道
11. 控制器管道
12. 路由管道
13. 路由参数管道
14. 控制器（方法处理器）
15. 路由拦截器（请求之后）
16. 控制器拦截器 （请求之后）
17. 全局拦截器 （请求之后）
18. 异常过滤器 （路由，之后是控制器，之后是全局）
19. 服务器响应

## 模块

模块是一个独立的应用单位，比如你可以将用户登录注册、配置项管理、商品定单管理分别定义为不同的模块

- 使用 imports 导入其他模块
- 使用 providers 属性向其他模块提供服务
- 使用 inject 属性注入其他模块提供的服务
- 使用 controllers 属性声明模块的控制器，以供路由识别
- 社区有众多 NestJs 的模块，我们可以拿来使用，比如 ConfigModule 配置管理模块
- 模块是单例的
- providers 定义的提供者也是单例的

### 根模块

每个应用程序至少有一个模块，即根模块。根模块是 Nest 用于构建 **应用程序图** 的起点。根模块在 **main.ts** 中定义。

如果你想让局域网用户可访问，可以在 **main.ts** 中指定 ip 地址

```typescript
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService]
})
export class AppModule {}

```

### 全局模块

使用 **@Global** 装饰器定义的模块为全局模块，其他模块在使用全局模块时不需要 **imports** 导入该模块

- **@Global** 装饰器将模块定义为全局作用域，其他模块不需要使用 **imports** 引入该模块就可以使用
- 全局模块也需要使用 **exports** 选项向其他模块提供接口
- 不建议将模块都定义为全局模块，但像配置模块是广泛使用的，可以定义为全局模块

```typescript
@Global()
@Module({
  providers: [AuthService],
  exports: [AuthService],
})
```



### **providers** 提供者服务注册

#### 默认推荐注册方式

```typescript
@Controller()
export class AppController {
  constructor(private readonly appService: AppService, @Optional private name:string) {}

  @Get('/q')
  getHello(): string {
    return this.appService.getHello();
  }
}
//===============================================================
@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService]
})
export class AppModule {}
```

#### 手动注册方式

```typescript
@Controller()
export class AppController {
  constructor(
    @Inject('hd')
    private readonly appService: AppService) {}

  @Get('/q')
  getHello(): string {
    return this.appService.getHello();
  }
}
//===============================================================
@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService,{
    provide: 'hd',
    useClass: AppService, //非类也相似，使用 useValue 键值
  }]
})
export class AppModule {}
```

#### 工厂函数注册

```typescript
import { Module } from '@nestjs/common'
import { AuthController } from './auth.controller'
import { AuthService } from './auth.service'

class XjClass {
  make() {
    return 'this is XjClass Make Method'
  }
}
@Module({
  providers: [
    AuthService,
    XjClass,
    {
      provide: 'HD',
      //依赖注入其他提供者,将做为参数传递给 useFactory 方法
      inject: [XjClass],
      useFactory: (xjClass) => {
        return xjClass.make()
      },
    },
  ],
  controllers: [AuthController],
})
export class AuthModule {}
```

#### 服务导入导出

```typescript
@Module({
  providers: [XjService],
  controllers: [XjController],
  exports: [XjService], //需要明确导出什么服务
})
export class XjModule {}
//=======================================================
@Module({
  imports: [XjModule], //别打模块导出的服务才可以调用
  controllers: [HdController],
  providers: [HdService],
})
export class HdModule {}
```

## 管道

管道 pipe 用于对控制器接收的数据进行验证或类型转换

### 内置管道 [](https://doc.houdunren.com/系统课程/NestJs/7 管道PIPE.html#内置管道)

`Nest` 自带九个开箱即用的 [内置管道](https://docs.nestjs.com/pipes#built-in-pipes)。

- `ValidationPipe`
- `ParseIntPipe`
- `ParseFloatPipe`
- `ParseBoolPipe`
- `ParseArrayPipe`
- `ParseUUIDPipe`
- `ParseEnumPipe`
- `DefaultValuePipe`
- `ParseFilePipe`

### 自定义管道

1. 创建管道文件

```shell
nest g pi custom
```

内容如下

```typescript
import { ArgumentMetadata, Injectable, PipeTransform } from '@nestjs/common';

@Injectable()
export class CustomPipe implements PipeTransform {
  transform(value: any, metadata: ArgumentMetadata) {
    return metadata.metatype == Number ? +value : value;
  }
}
```



### 声明使用方式

管道可以使用以下方式声明使用, 位置不同作用的目标也不同

#### **控制器**

控制器

```typescript
@UsePipes(ValidationPipe)
export class UserController{
}
```

控制器方法

```typescript
@UsePipes(ValidationPipe)
show()
```

方法参数

```typescript
show(@param('id',ParseIntPipe))
```

#### 模块

一般用于声明对模块全局影响的管道如表单验证

```typescript
...
import { ValidatePipe } from './validate.pipe';
import { APP_PIPE } from '@nestjs/core';

@Module({
  imports: [],
  controllers: [AppController],
  providers: [
    AppService,
    {
      provide: APP_PIPE,
      useClass: ValidatePipe,
    },
  ],
})
export class AppModule {}
```

#### 全局管道

一般用于声明对模块全局影响的管道如表单验证

```typescript
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(new ValidationPipe());
  await app.listen(3000, '0.0.0.0');
}
```

## 数据验证

### 自定义管道验证(缺乏复用性)

- 管道文件

  ```typescript
  import { ArgumentMetadata, BadRequestException, Injectable, PipeTransform } from '@nestjs/common';
  
  @Injectable()
  export class ValitePipe implements PipeTransform {
    transform(value: any, metadata: ArgumentMetadata) {
      if (value.content == '') throw new BadRequestException('内容不能为空');
      return value;
    }
  }
  ```

- 使用

  ```typescript
  import { Controller, Inject, Post, Body } from '@nestjs/common';
  import { ValitePipe } from './valite/valite.pipe';
  
  @Controller()
  export class AppController {
    constructor() {}
  
    @Post('login')
    login(@Body(ValitePipe) mobile: Record<string, any>) {
      return mobile;
    }
  }
  
  ```

### 使用工具验证

- 安装依赖包

  ```shell
  pnpm add class-validator class-transformer
  ```

- 创建验证文件 dto (Data Transfer Object)  文件 user.dto.ts

  ```typescript
  import { IsNotEmpty } from 'class-validator';
  export class UserDto {
    @IsNotEmpty({message: '$property:用户名不能为空'})
    name: string;
    //存在price时才验证
    @ValidateIf((o) => o.price)
    //将类型转换为数值
    @Type(() => Number)
    price:number
  }
  ```

- 或使用现成管道验证

  ```typescript
  import { NestFactory } from '@nestjs/core';
  import { AppModule } from './app.module';
  import { ValidationPipe } from '@nestjs/common';
  
  export async function bootstrap() {
    const app = await NestFactory.create(AppModule);
    app.useGlobalPipes(new ValidationPipe());
    await app.listen(3000);
  }
  
  ```

  

- 创建管道文件验证

  ```typescript
  import { ArgumentMetadata, BadRequestException, Injectable, PipeTransform } from '@nestjs/common';
  import { plainToInstance } from 'class-transformer';
  import { validate } from 'class-validator';
  
  @Injectable()
  export class ValitePipe implements PipeTransform {
    async transform(value: any, metadata: ArgumentMetadata) {
      const { metatype } = metadata;
      //前台提交的表单数据没有类型，使用 plainToClass 转为有类型的对象用于验证
      const object = plainToInstance(metatype, value);
  
      //根据 DTO 中的装饰器进行验证
      const errors = await validate(object);
        if (err.length) {
        const messages = err.map((item) => ({
          name: item.property,
          messages: item.constraints ? Object.values(item.constraints).map((i) => i) : ''
        }));
  
        throw new HttpException(messages, HttpStatus.BAD_REQUEST);
      }
      return value;
    }
  }
  
  ```

- 在控制器中使用

  ```typescript
  @Post('add')
  add(@Body(ValidatePipe) dto: UserDto): any {
    return dto;
  }
  ```

  

#### 自定义错误格式

我们可以对响应的消息进行自定义处理，方便前端 vue/react 的使用

- 定义类

  ```typescript
  import { HttpException, HttpStatus, ValidationError, ValidationPipe } from '@nestjs/common'
  
  export default class ValidatePipe extends ValidationPipe {
    protected flattenValidationErrors(validationErrors: ValidationError[]): string[] {
      const messages = validationErrors.map((error) => {
        return { field: error.property, message: Object.values(error.constraints)[0] }
      })
  
      throw new HttpException(
        {
          code: HttpStatus.BAD_REQUEST,
          messages,
          error: 'bad request',
        },
        HttpStatus.BAD_REQUEST,
      )
    }
  }
  ```

- 管道注册

  ```typescript
  import { NestFactory } from '@nestjs/core'
  import { AppModule } from './app.module'
  import { ValidateExceptionFilter } from './common/exceptions/validate.exception'
  import Validate from './common/rules/ValidatePipe'
  
  async function bootstrap() {
    const app = await NestFactory.create(AppModule)
    app.useGlobalPipes(new ValidatePipe())
    await app.listen(3000)
  }
  bootstrap()
  ```

#### 自定义验证规则

##### 类定义

```typescript
import { PrismaClient } from '@prisma/client';
import {
  ValidationArguments,
  ValidatorConstraint,
  ValidatorConstraintInterface,
} from 'class-validator';

@ValidatorConstraint()
export class IsConfirmedRule implements ValidatorConstraintInterface {
  async validate(value: string, args: ValidationArguments) {
    return value == args.object[`${args.property}_confirmation`]; //返回true为成功
  }

  defaultMessage(args: ValidationArguments) { //消息
    return '数据不匹配';
  }
}
```

使用

```typescript
import { IsNotEmpty,Validate } from 'class-validator'
import { IsConfirmedRule } from 'src/rules/is.confirmed.rule'

export class RegisterDto {
  @IsNotEmpty()
  @Validate(IsConfirmedRule,{ message: '两次密码不一致' })
  password: string
}
```

##### 装饰器定义

```typescript
import {
  registerDecorator,
  ValidationArguments,
  ValidationOptions,
} from 'class-validator';

//表字段是否唯一
export function IsConfirmedRule(validationOptions?: ValidationOptions) {
  return function (object: Record<string, any>, propertyName: string) {
    registerDecorator({
      name: 'IsConfirmedRule',
      target: object.constructor,
      propertyName: propertyName,
      constraints: [],
      options: validationOptions,
      validator: {
        validate(value: string, args: ValidationArguments) {
          return value == args.object[`${args.property}_confirmation`]; //返回true代表成功
        },
      }
    });
  };
}
```

使用

```typescript
import { IsNotEmpty } from 'class-validator'
import { IsConfirmedRule } from 'src/rules/is.confirmed.rule'

export class RegisterDto {
  @IsConfirmedRule({ message: '两次密码不一致' })
  @IsNotEmpty()
  password: string
}
```

#### 其它配置

**ValidationPipe** 可以根据对象的 **DTO** 类自动将有效数据转换为对应类型。

```typescript
const app = await NestFactory.create(AppModule);
app.useGlobalPipes(
  new ValidationPipe({
    transform: true,
    whitelist: true, //过滤多余的数据
  }),
);
```

