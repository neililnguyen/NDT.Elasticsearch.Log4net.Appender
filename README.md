# NDT.Elasticsearch.Log4net.Appender
Solution to send log to Elasticsearch by Log4net library

Target Framework:
  - > ">= .netFramework 4.6.1"
  - > ">= .NetCore 2.0"
  
Tutorial:
1. Install package: Install-Package NDT.Elasticsearch.Log4net.Appender -Version 1.0.0
2. Config:
    
 a. NetFramework:
    
  - In Web.config:
    
        <configSections>
          <section name="log4net" type="log4net.Config.Log4NetConfigurationSectionHandler, log4net" />
        </configSections>
        <appSettings>
          <add key="log4net.Internal.Debug" value="true" />
        </appSettings>
        <log4net>
          <appender name="elasticappender" type="demo_lib_log4net_appender.Appender.UsexpressAppender, demo_lib_log4net_appender">
                <elasticNode value="http://elastic:changeme@localhost:9200" />
                <enableGlobalContextLog value="false" />
                <disableLocationInfo value="false" />
                <disableConnectionPing value="false" />
                <indexPattern value="ddMMyyyy" />
                <baseIndex value="logentry" />
          </appender>
          <root>
                <level value="ALL" />
                <appender-ref ref="elasticappender" />
          </root>
        </log4net>
      
     - In AssemblyInfo.cs
      	
            [assembly: log4net.Config.XmlConfigurator(Watch = true)]
    
b. netCore:
    
  - Create log4net.config:
     
         <log4net>
            <appender name="elasticappender" type="demo_lib_log4net_appender.Appender.UsexpressAppender, demo_lib_log4net_appender">
                <elasticNode value="http://elastic:changeme@localhost:9200"></elasticNode>
                <enableGlobalContextLog value="true"></enableGlobalContextLog>
                <disableLocationInfo value="false"></disableLocationInfo>
                <disableConnectionPing value="false"></disableConnectionPing>
                <indexPattern value="yyyy-MM-dd"></indexPattern>
                <baseIndex value="demo_elastic_log4net_"></baseIndex>
            </appender>

            <root>
              <level value="ALL" />
              <appender-ref ref="elasticappender" />
            </root>
         </log4net>
     
  - In Startup.cs, add this code in contructor:
     
          XmlConfigurator.Configure(LogManager.GetRepository(Assembly.GetEntryAssembly()), new FileInfo("log4net.config"));
          
 c. Note:
    
   - http://elastic:changeme@localhost:9200 : elastic is default username, changeme is default password
     
   - <baseIndex value="demo_elastic_log4net_"></baseIndex> : demo_elastic_log4net_ is my index
     
3. Using:

  - Logging.log.Info("Your message hear");
  - Logging.log.Info(JsonConvert.SerializeObject(object)); //Install Newtonsoft.Json to use it
       
       
