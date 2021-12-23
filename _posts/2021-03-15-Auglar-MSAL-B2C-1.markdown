---
layout: post
title: Angular 11 MSAL B2C登录实例(一) 
date: 2021-03-15 16:07:24.000000000 +09:00
---

**关键词** Angular 11 B2C MSAL

### 前言
因为项目需求，需要把Angular 11项目中登录方式改成B2C登录，所以在参考了一系列文档后，成功通过MSAL将项目的登录方式改成B2C登录。下面介绍了详细步骤及一些注意事项。

### 步骤：
#### 1. 安装MSAL
在项目中安装msal
`npm i @azure/msal-angular --save`
`npm i @azure/msal-browser --save`
通过查阅MSAL的文档，发现v2以上版本才支持Angualr11，所以在本项目的代码中使用的是:
`"@azure/msal-angular": "^2.0.3", ` 
` "@azure/msal-browser": "^2.17.0",`

#### 2. 引入MSAL
在项目中引入MSAL主要是在两个文件中引入，app.module.ts和app-routing.module.ts中引入，下面先讲解下app.module.ts这个文件。
> app.module.ts

{% highlight ruby %}
...
import { IPublicClientApplication, PublicClientApplication, InteractionType, BrowserCacheLocation, LogLevel } from '@azure/msal-browser';
import { MsalInterceptor, MsalBroadcastService, MsalInterceptorConfiguration, MsalModule, MsalService, MSAL_INSTANCE, MSAL_INTERCEPTOR_CONFIG, MsalRedirectComponent } from '@azure/msal-angular';

export function loggerCallback(logLevel: LogLevel, message: string) {
}

export function MSALInstanceFactory(): IPublicClientApplication {
  return new PublicClientApplication({
    auth: {
    clientId: "your-client-id",
    authority: "your-authority",
    knownAuthorities: "your-knownAuthorities", 
    redirectUri: "your-redirectUri", 
    postLogoutRedirectUri: "your-LogoutRedirectUri:"
    
  }
    cache: {
      cacheLocation: BrowserCacheLocation.SessionStorage,
      storeAuthStateInCookie: false, 
    },
    system: {
      loggerOptions: {
        loggerCallback,
        logLevel: LogLevel.Info,
        piiLoggingEnabled: false
      }
    }
  });
}

export function MSALInterceptorConfigFactory(): MsalInterceptorConfiguration {
  const protectedResourceMap = new Map<string, Array<string>>();
  protectedResourceMap.set("your-local-set", [yourB2CScope]);

  return {
    interactionType: InteractionType.Redirect,
    protectedResourceMap
  };
}

@NgModule({
  declarations: [
    ...
  ],
  imports: [
    ...
    MsalModule
  ],
  providers: [
    ...
    {
      provide: HTTP_INTERCEPTORS,
      useClass: MsalInterceptor,
      multi: true
    },
    {
      provide: MSAL_INSTANCE,
      useFactory: MSALInstanceFactory
    },
    {
      provide: MSAL_INTERCEPTOR_CONFIG,
      useFactory: MSALInterceptorConfigFactory
    },
    MsalService,
    MsalBroadcastService
  ],
  bootstrap: [AppComponent, MsalRedirectComponent]
})
}
{% endhighlight %}
> 注意： 在以上实例中，your开头的，都是需要自己配置的信息。 省略号代表着你自己项目中的其他代码。

### 后言
下一篇文章会讲解在app-routing里的配置


