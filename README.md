# valet-default-conf

https://github.com/play-valet/valet

```
scaffold {

  # db setting
  db {
    db: "mysql"
    isUseDocker: "YES"
    default.driver: "com.mysql.jdbc.Driver"
    default.url: "jdbc:mysql://127.0.0.1/test?characterEncoding=UTF8&autoReconnect=true&useSSL=false"
    default.username: "test"
    default.password: "test"
  }
  
  # relation:
  #   "SINGLE": scaffold "crud one table"
  #   "ONE_TO_MANY": scaffoled "crud one table ralated other table's column". ex. crud blog by login user
  table {
    tableList: [
      {
        tableName: "User"
        relation: "SINGLE"
        isScaffoldList: ["dao"]
        isRollList: [""]
        columnList: [
          {cName: "user_id", cType: "Int", cAttr: ["AUTO_INCREMENT", "PRIMARY_KEY", "UNIQUE"]},
          {cName: "user_auth_text", cType: "String", cAttr: []},
          {cName: "user_auth_hash", cType: "String", cAttr: []},
          {cName: "user_nickname", cType: "String", cAttr: ["UNIQUE"]},
          {cName: "user_email", cType: "String", cAttr: ["UNIQUE"]},
          {cName: "user_roll_id", cType: "Int", cAttr: ["DEFAULT:21"]},
          {cName: "user_timezone", cType: "String", cAttr: ["DEFAULT:'Asia/Tokyo'"]},
          {cName: "user_language", cType: "String", cAttr: ["DEFAULT:'ja'"]}
        ]
      },
      {
        tableName: "ErrorLog"
        relation: "SINGLE"
        isScaffoldList: ["dao"]
        isRollList: [""]
        columnList: [
          {cName: "errorlog_id", cType: "Int", cAttr: ["AUTO_INCREMENT", "PRIMARY_KEY", "UNIQUE"]},
          {cName: "user_id", cType: "Option[Int]", cAttr: ["FOREIGN:User,user_id", "NO_VIEW_MODEL", "FORM_MAPPING:LOGGEDIN"]},
          {cName: "errorlog_content", cType: "String", cAttr: []},
          {cName: "errorlog_modified", cType: "DateTime", cAttr: ["NO_VIEW_MODEL"]}
        ]
      },
      {
        tableName: "Posts"
        relation: "SINGLE"
        isScaffoldList: ["controller", "form", "businesslogic", "service", "repository", "dao", "view"]
        isRollList: [""]
        columnList: [
          {cName: "posts_id", cType: "Int", cAttr: ["AUTO_INCREMENT", "PRIMARY_KEY", "UNIQUE"]},
          {cName: "user_id", cType: "Option[Int]", cAttr: ["FOREIGN,User,user_id", "NO_VIEW_MODEL", "FORM_MAPPING:LOGGEDIN"]},
          {cName: "posts_author", cType: "String", cAttr: []},
          {cName: "posts_title", cType: "Option[String]", cAttr: []},
          {cName: "posts_content", cType: "Option[String]", cAttr: []},
          {cName: "posts_modified", cType: "DateTime", cAttr: ["NO_VIEW_MODEL"]}
        ]
      },
      {
        tableName: "Campaign"
        relation: "SINGLE"
        isScaffoldList: ["controller", "form", "service", "dao", "view"]
        isRollList: ["showIndex:Admin", "showDetail:Admin", "showCreate:Admin", "store:Admin", "showEdit:Admin", "update:Admin", "destroy:Admin"]
        columnList: [
          {cName: "campaign_id", cType: "Int", cAttr: ["AUTO_INCREMENT", "PRIMARY_KEY", "UNIQUE"]},
          {cName: "user_id", cType: "Option[Int]", cAttr: ["FOREIGN,User,user_id", "NO_VIEW_MODEL", "FORM_MAPPING:LOGGEDIN"]},
          {cName: "campaign_author", cType: "String", cAttr: []},
          {cName: "campaign_title", cType: "String", cAttr: []},
          {cName: "campaign_content", cType: "Option[String]", cAttr: []},
          {cName: "campaign_modified", cType: "DateTime", cAttr: ["NO_VIEW_MODEL"]}
        ]
      }
    ]
  }

  # directory.type:
  #   "AG": generate Autogen-Dir-Model. ignore output-path-config.
  #   "MVC": simple output according to output-path-config.
  # options
  #   "CONTROLLER:ASYNC":
  #   "CONTROLLER:SYNC":
  #   "CONTROLLER:EITHERT":
  general {
    output {
      directory.type: "AG"
      path {
        slick.tables: "app/models/autogen/tables/Tables.scala"
        controller: ""
        dao: ""
        service: ""
        form: ""
        view: ""
      }
      options: ["CONTROLLER:ASYNC"]
    }
  }

  # modules setting
  modules {
    required {
      dbmigration {
        isUse: "YES"
        library: "flyway"
        path : "conf/db/migration/default"
      }
      slickcodegen {
        isUse: "YES"
        library: "default"
      }
    }

    # how to define https://www.howto/~
    auth {
      isUse: "NO"
      library: "jp.t2v.lab.play2.auth"
      roleList: ["SuperAdmin:5", "Admin:8", "SuperUser:13", "User:21", "SuperGuest:34", "Guest:55"]
      table {
        accountTableName: "User"
        LoginIdColumnName: "user_auth_text"
        LoginPassColumnName: "user_auth_hash"
      }
    }

    # how to define https://www.howto/~
    i18nCrudTable {
      isUse: "NO"
      i18nList: ["ja", "en", "zh-cn", "zh-tw", "fr", "ar", "it", "nl", "ko", "es", "th", "de", "vi", "ms", "ru"]
      table {
        tableName: ""
      }
    }

    # how to define https://www.howto/~
    i18nMessageConf {
      isUse: "YES"
      i18nList: ["ja", "en", "zh-cn", "zh-tw", "fr", "ar", "it", "nl", "ko", "es", "th", "de", "vi", "ms", "ru"]
    }

    # how to define https://www.howto/~
    twirlScaffoldThemes {
      isUse: "YES"
      enableList: ["backend.admin"]
      source {
        front.blog: ""
        front.corporate: ""
        backend.admin: "git@github.com:play-valet-themes/simple-admin.git"
        account: ""
      }
      modules {
        resultDto {
          isUse: "YES"
          name: "ResultDtos"
        }
      }
    }
  }
}
```
