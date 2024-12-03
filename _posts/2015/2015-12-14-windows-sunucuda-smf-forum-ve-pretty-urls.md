---
layout: post
title: "Windows Sunucuda SMF Forum ve Pretty URLs"
date: 2015-12-14 09:54
categories: ["Yazilim", "Windows Server"]
tags: ["windows", "smf forum", "pretty url", "plugin"]
toc: true
---

SMF Forum ile birlikte Pretty URLs eklentisini windows sunucu üzerinde çalıþtırmak istiyorsanız apache rule´larının çalıştırmadığını ve sistemin hata verdiğini göreceksiniz. Bunu çözümlemek aslında çok kolay fakat biraz profesyonel bilgi gerektiriyor.

Bilginin paylaştıkça çoğaldığına inananlardanım biliyorsunuz :) Bu sebeple, sorununuzu çözmek için 2 farklı method anlatacağım. Tam yetkili yada yetkili değilseniz size uygun yöntem ile SMF Forumu windows hosting üstünde Pretty URLs eklentisi ile SEO dostu bir şekilde çalıştırabilirsiniz.

## 1. IIS Manager Üzerinden Rule Import
Eğer sunucu erişiminiz varsa, aşağıda ki adımları uygulayarak ilgili ayarları yapabilirsiniz.

- Sunucu üzerinde IIS Manager´ı çalıştırın
- ilgili domaini bulun
- URL Rewrite butonuna tıklayın
- Sağ bölümde Inbound Rule bölümüne tıklayın
- Açılan alanda kod bölümüne aşağıda ki kodları yazın ve kaydedin.

```bash
RewriteCond %{REQUEST_FILENAME} !-f</code>

RewriteCond %{REQUEST_FILENAME} !-d

RewriteRule ^(.*)$ index.php/$1 [L]
```

## 2. web.config Dosyayı Yükleyerek
Aşağıda ki kodu web.config adlı bir dosya oluşturarak yada var olan dosyayı değiştirerek ana dizine (httpdocs, www vb) atın.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <configSections>
        <sectionGroup name="system.web.extensions" type="System.Web.Configuration.SystemWebExtensionsSectionGroup, System.Web.Extensions, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35">
            <sectionGroup name="scripting" type="System.Web.Configuration.ScriptingSectionGroup, System.Web.Extensions, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35">
                <section name="scriptResourceHandler" type="System.Web.Configuration.ScriptingScriptResourceHandlerSection, System.Web.Extensions, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" requirePermission="false" allowDefinition="MachineToApplication" />
                <sectionGroup name="webServices" type="System.Web.Configuration.ScriptingWebServicesSectionGroup, System.Web.Extensions, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35">
                    <section name="jsonSerialization" type="System.Web.Configuration.ScriptingJsonSerializationSection, System.Web.Extensions, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" requirePermission="false" allowDefinition="Everywhere" />
                    <section name="profileService" type="System.Web.Configuration.ScriptingProfileServiceSection, System.Web.Extensions, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" requirePermission="false" allowDefinition="MachineToApplication" />
                    <section name="authenticationService" type="System.Web.Configuration.ScriptingAuthenticationServiceSection, System.Web.Extensions, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" requirePermission="false" allowDefinition="MachineToApplication" />
                    <section name="roleService" type="System.Web.Configuration.ScriptingRoleServiceSection, System.Web.Extensions, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" requirePermission="false" allowDefinition="MachineToApplication" />
                </sectionGroup>
            </sectionGroup>
        </sectionGroup>
    </configSections>
    <appSettings />
    <connectionStrings />
    <system.web>
        <!-- 
         Set compilation debug="true" to insert debugging 
         symbols into the compiled page. Because this 
         affects performance, set this value to true only 
         during development.
         -->
        <compilation debug="true">
            <assemblies>
                <add assembly="System.Core, Version=3.5.0.0, Culture=neutral, PublicKeyToken=B77A5C561934E089" />
                <add assembly="System.Data.DataSetExtensions, Version=3.5.0.0, Culture=neutral, PublicKeyToken=B77A5C561934E089" />
                <add assembly="System.Web.Extensions, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" />
                <add assembly="System.Xml.Linq, Version=3.5.0.0, Culture=neutral, PublicKeyToken=B77A5C561934E089" />
            </assemblies>
        </compilation>
        <!--
         The <authentication> section enables configuration 
         of the security authentication mode used by 
         ASP.NET to identify an incoming user. 
         -->
                <authentication mode="Windows" />
                <!--
         The <customErrors> section enables configuration 
         of what to do if/when an unhandled error occurs 
         during the execution of a request. Specifically, 
         it enables developers to configure html error pages 
         to be displayed in place of a error stack trace.

         <customErrors mode="RemoteOnly" defaultRedirect="GenericErrorPage.htm">
         <error statusCode="403" redirect="NoAccess.htm" />
         <error statusCode="404" redirect="FileNotFound.htm" />
         </customErrors>
         -->
        <pages>
            <controls>
                <add tagPrefix="asp" namespace="System.Web.UI" assembly="System.Web.Extensions, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" />
                <add tagPrefix="asp" namespace="System.Web.UI.WebControls" assembly="System.Web.Extensions, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" />
            </controls>
        </pages>
        <httpHandlers>
            <remove verb="*" path="*.asmx" />
            <add verb="*" path="*.asmx" validate="false" type="System.Web.Script.Services.ScriptHandlerFactory, System.Web.Extensions, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" />
            <add verb="*" path="*_AppService.axd" validate="false" type="System.Web.Script.Services.ScriptHandlerFactory, System.Web.Extensions, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" />
            <add verb="GET,HEAD" path="ScriptResource.axd" type="System.Web.Handlers.ScriptResourceHandler, System.Web.Extensions, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" validate="false" />
        </httpHandlers>
        <httpModules>
            <add name="ScriptModule" type="System.Web.Handlers.ScriptModule, System.Web.Extensions, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" />
        </httpModules>
    </system.web>
    <system.codedom>
        <compilers>
            <compiler language="c#;cs;csharp" extension=".cs" warningLevel="4" type="Microsoft.CSharp.CSharpCodeProvider, System, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089">
                <providerOption name="CompilerVersion" value="v3.5" />
                <providerOption name="WarnAsError" value="false" />
            </compiler>
        </compilers>
    </system.codedom>
    <!-- 
     The system.webServer section is required for running ASP.NET AJAX under Internet
     Information Services 7.0. It is not necessary for previous version of IIS.
     -->
    <system.webServer>
        <validation validateIntegratedModeConfiguration="false" />
        <modules>
            <remove name="ScriptModule" />
            <add name="ScriptModule" preCondition="managedHandler" type="System.Web.Handlers.ScriptModule, System.Web.Extensions, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" />
        </modules>
        <handlers>
            <remove name="WebServiceHandlerFactory-Integrated" />
            <remove name="ScriptHandlerFactory" />
            <remove name="ScriptHandlerFactoryAppServices" />
            <remove name="ScriptResource" />
            <add name="ScriptHandlerFactory" verb="*" path="*.asmx" preCondition="integratedMode" type="System.Web.Script.Services.ScriptHandlerFactory, System.Web.Extensions, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" />
            <add name="ScriptHandlerFactoryAppServices" verb="*" path="*_AppService.axd" preCondition="integratedMode" type="System.Web.Script.Services.ScriptHandlerFactory, System.Web.Extensions, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" />
            <add name="ScriptResource" preCondition="integratedMode" verb="GET,HEAD" path="ScriptResource.axd" type="System.Web.Handlers.ScriptResourceHandler, System.Web.Extensions, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" />
        </handlers>
        <rewrite>
            <rules>
                <rule name="Imported Rule 1" stopProcessing="true">
                    <match url="^(.*)$" ignoreCase="false" />
                    <conditions logicalGrouping="MatchAll">
                        <add input="{REQUEST_FILENAME}" matchType="IsFile" ignoreCase="false" negate="true" />
                        <add input="{REQUEST_FILENAME}" matchType="IsDirectory" ignoreCase="false" negate="true" />
                    </conditions>
                    <action type="Rewrite" url="index.php/{R:1}" />
                </rule>
            </rules>
        </rewrite>
    </system.webServer>
    <runtime>
        <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
            <dependentAssembly>
                <assemblyIdentity name="System.Web.Extensions" publicKeyToken="31bf3856ad364e35" />
                <bindingRedirect oldVersion="1.0.0.0-1.1.0.0" newVersion="3.5.0.0" />
            </dependentAssembly>
            <dependentAssembly>
                <assemblyIdentity name="System.Web.Extensions.Design" publicKeyToken="31bf3856ad364e35" />
                <bindingRedirect oldVersion="1.0.0.0-1.1.0.0" newVersion="3.5.0.0" />
            </dependentAssembly>
        </assemblyBinding>
    </runtime>
</configuration>
```