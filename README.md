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

  # general setting
  general {
    slick.path.tables: "app/models/autogen/tables/Tables.scala"
    output {
      # directory.type:
      #   "AG": generate Autogen-Dir-Model. ignore output-path-config.
      #   "MVC": simple output according to output-path-config.
      directory.type: "AG"
      path {
        controller: ""
        dao: ""
        service: ""
        form: ""
        view: ""
      }
      # options
      #   "CONTROLLER:ASYNC":
      #   "CONTROLLER:SYNC":
      #   "CONTROLLER:EITHERT":
      options: ["CONTROLLER:EITHERT"]
    }
  }

  # modules setting
  modules {
    required {
      # how to define https://www.howto/~
      dbmigration {
        isUse: "YES"
        library: "flyway"
        path {
          migration: "conf/db/migration/default"
        }
      }
      # how to define https://www.howto/~
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
        front {
          blog: ""
          corporate: "git@github.com:twirl-component-themes/tct-corporate.git"
          statusErrors: ""
        }
        backend {
          admin: "git@github.com:twirl-scaffold-themes/tst-simple-admin.git"
        }
        account: "git@github.com:twirl-component-themes/tct-login.git"
      }
      SingleDtoBetweenViewAndController {
        isUse: "YES"
        name: "ResultDtos"
      }
    }

  }

  # relation:
  #   "SINGLE": scaffold "crud one table"
  #   "ONE_TO_MANY": scaffoled "crud one table ralated other table's column". ex. crud blog by login user
  #   "ONE_TO_MANY_I18N": scaffold "crud tables ralated other table's column". ex. crud blog for i18n by login user
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
}

```
