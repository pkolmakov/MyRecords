# Configuring JAVA environment for neovim
- install java sdk
- download maven https://maven.apache.org/download.cgi and register PATH to bin
    - then use https://mkyong.com/maven/how-to-create-a-java-project-with-maven/
    - https://maven.apache.org/guides/introduction/introduction-to-the-pom.html
      it's documentation for pom.xml
- download from https://download.eclipse.org/jdtls/snapshots/ the latest and
  unzip it to any folder that you'll use
- add to nvim config: 
    - plug 'prabirshrestha/vim-lsp' 
    - plug 'mattn/vim-lsp-settings'
    - Plug 'mfussengger/nvim-jdtls'
- check nvim-jdtls and create ftplugin folder with java.lua and replace needed
  urls
- call CocInstall coc-java 
    - call CocConfig and add there "java.home":"C:/Program Files/Java/jdk-18.0.1.1"
    (Pay attention that if you place some wrong settings additionaly you'll get
    loading server Errors)
    - in case of error like 'can't start server' 
        - clear coc config file from wrong settings
        - copy from downloaded by you Latest jdtls to
          coc/Extensions/data/server/...
## Configuring with sqlserver
- download jdbc driver for sqlserver
- put to classpath (as environment name CLASSPATH) or directly in place where my
  app lies;
- copy auth...dll to Java/bin and Java/lib 
- add to resourse/application.properties
    spring.datasource.url=jdbc:sqlserver://Host_Name;encrypt=false;databaseName=Schema_name;integratedSecurity=true;

## Configuring debugging

- Coc plugin should be installed
- CocInstall coc-java and coc-java-debug
- create configuration file in root of app CocConfig
>    {
>  "adapters": {
>    "java-debug-server": {
>      "name": "vscode-java",
>      "port": "${AdapterPort}"
>    }
>  },
>  "configurations": {
>    "Java Attach": {
>      "default": true,
>      "adapter": "java-debug-server",
>      "configuration": {
>        "request": "attach",
>        "host": "127.0.0.1",
>        "port": "5005"
>      },
>      "breakpoints": {
>        "exception": {
>          "caught": "N",
>          "uncaught": "N"
>        }
>      }
>    }
>  }
>}
 - compile app
 - run app 
 > java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005
 > fullPathToClass
