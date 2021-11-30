<template>
  <v-app>
    
    <v-app-bar 
      color="blue"
      dense
      app
    >
      
      <v-menu offset-y>
                <template v-slot:activator="{ on }">
                    <v-btn v-on="on" color="primary" left>
                        <span>Tables</span>
                    </v-btn>
                </template>
                <v-list>
                    <v-list-item v-for="item in menuitems" :key="item.text"
                    @click="selectSection(item.text)">
                        <v-list-item-title>{{ item.text }}</v-list-item-title>
                    </v-list-item>
                </v-list>
      </v-menu>
    </v-app-bar>
    
    

    
    <v-main>
        <v-data-table
        :headers="switch_headers"
        :items="switch_data"
        :items-per-page="5"
        class="elevation-1"
        ></v-data-table>
      <div>
        <h3>User create/update (вводить все данные через запятую(a,b,c,d,e,True,f,1,9))</h3>
        <v-text-field lable="UserCU" v-model="tempuser" prepend-icon="mdi-account-plus"></v-text-field>
        <h3>User delete (вводить id пользователя)</h3>
        <v-text-field lable="UserD" v-model="iddeleteuser" prepend-icon="mdi-delete"></v-text-field>
      </div>
      <v-menu offset-y>
                <template v-slot:activator="{ on }">
                    <v-btn v-on="on" color="primary" left>
                        <span>Actions</span>
                    </v-btn>
                </template>
                <v-list>
                    <v-list-item v-for="action in actions" :key="action.text"
                    @click="selectActionUser(action.text)">
                        <v-list-item-title>{{ action.text }}</v-list-item-title>
                    </v-list-item>
                </v-list>
      </v-menu>
      <div>
        <h3>Role create/update (вводить все данные через запятую(a,b)</h3>
        <v-text-field lable="RoleCU" v-model="temprole" prepend-icon="mdi-account-plus"></v-text-field>
        
        <h3>Role delete (вводить id пользователя)</h3>
        <v-text-field lable="RoleD" v-model="iddeleterole" prepend-icon="mdi-delete"></v-text-field>
      </div>
      <v-menu offset-y>
                <template v-slot:activator="{ on }">
                    <v-btn v-on="on" color="primary" left>
                        <span>Actions</span>
                    </v-btn>
                </template>
                <v-list>
                    <v-list-item v-for="action in actions" :key="action.text"
                    @click="selectActionRole(action.text)">
                        <v-list-item-title>{{ action.text }}</v-list-item-title>
                    </v-list-item>
                </v-list>
      </v-menu>
      <div>
        <h3>UserRole create (вводить id нужного пользователя)</h3>
        <v-text-field lable="UserRoleC" v-model="tempuserrole" prepend-icon="mdi-account-plus"></v-text-field>
        
        <h3>UserRole delete (вводить id нужного пользователя)</h3>
        <v-text-field lable="UserRoleD" v-model="iddeleteuserrole" prepend-icon="mdi-delete"></v-text-field>
      </div>
      <v-menu offset-y>
                <template v-slot:activator="{ on }">
                    <v-btn v-on="on" color="primary" left>
                        <span>Actions</span>
                    </v-btn>
                </template>
                <v-list>
                    <v-list-item v-for="action in actionsuserrole" :key="action.text"
                    @click="selectActionUserRole(action.text)">
                        <v-list-item-title>{{ action.text }}</v-list-item-title>
                    </v-list-item>
                </v-list>
      </v-menu>
    </v-main>
    
  </v-app>
</template>

<script>

export default {
  name: 'App',

  components: {
    
  },

  data: () =>{ 
    return {
      headers_users:[//неименования столбцов в таблице пользователи
        { text:"Name",value: "name"  },
        { text:"Username", value:"username" },
        { text:"Mail", value:"mail" },
        { text:"Password", value:"password" },
        { text:"Updated", value:"updated" },
        { text:"Is active", value:"is_active" },
        { text:"Created", value:"created" },
        { text:"Role ID", value:"role_id" },
        { text:"ID", value:"id" }
      ],
      headers_roles:[//наименование столбцов в таблице роли
        {text:"Name",value:"name"},
        {text:"ID",value:"id"}
      ],
      headers_userroles:[//наименование столбцов в таблице userroles
        { text:"Name",value: "name"  },
        { text:"Username", value:"username" },
        { text:"Mail", value:"mail" },
        { text:"Password", value:"password" },
        { text:"Updated", value:"updated" },
        { text:"Is active", value:"is_active" },
        { text:"Created", value:"created" },
        { text:"UserRole", value:"role" },
        { text:"ID", value:"id" }
      ],
      roles: [//переменная для хранения ролей
        { name: String, id: Number }
      ],
      users: [//переменная для хранения пользователей
        { name: String,username: String, mail: String, password: String, updated: String, is_active: Boolean,
        created: String, role_id: Number, id: Number }
      ],
      userrole: [//переменная для хранения данных таблицы userrole
        { name: String,username: String, mail: String, password: String, updated: String, is_active: Boolean,
        created: String, role: String, id: Number }
      ],
      menuitems: [{text:'Users'},{text:'Roles'},{text:'UserRoles'}],//элементы меню таблиц
      switch_headers: [], //переменная для хранения названий столбцов выбранной таблицы
      switch_data: [], //переменная для хранения данных выбранной таблицы
      newUser: //переменная для хранения данных о новом пользователе
        { name: String,username: String, mail: String, password: String, updated: String, is_active: Boolean,
        created: String, role_id: Number, id: Number },
      newRole://данные о новой роли
      {name: String, id: Number },
      iddeleterole: '',//id удаляемой роли
      temprole: '',//переменная для обработки вводимых данных о роли
      tempuser: '',//переменная для обработки вводимых данных о пользователе
      tempuserrole:'',//переменная для обработки вводимых данных о userrole
      actions: [{text:'Create'},{text:'Update'},{text:'Delete'}],//массив для меню действий
      actionsuserrole: [{text:'Create'},{text:'Delete'}],//массив для меню дейтсвий таблицы UserRole
      iddeleteuser: '',//переменная дя хранения id пользователя которого надо удалить
      iddeleteuserrole: '' //id удаляемой userrole
  }
  },
  mounted() {//первичное получение данных чрез api
    fetch("http://127.0.0.1:8000/users")//get
      .then(response => response.json())
      .then(json => {
        this.users = json//полученные данные в перменную users
      })
    fetch("http://127.0.0.1:8000/roles")
      .then(response => response.json())
      .then(json => {
        this.roles = json
      })
    fetch("http://127.0.0.1:8000/userroles")
      .then(response => response.json())
      .then(json => {
        this.userrole = json
      })
  },
  methods:{
    selectSection(text) { //функция для определения заголовков и данных через кнопку меню
      switch (text) {
          case 'Users'://если выбрана таблица пользователи
            this.fetchGetUsers()//получаем данные таблицы
            this.switch_headers = this.headers_users.map((x) => x)//заменяем заголовки в data table
            this.switch_data = this.users.map((x) => x) //заменяем данные в data table
            break
          case 'Roles':
            this.fetchGetRoles()
            this.switch_headers = this.headers_roles.map((x) => x)
            this.switch_data = this.roles.map((x) => x)
            break
          case 'UserRoles':
            this.fetchGetUserRoles()
            this.switch_headers = this.headers_userroles.map((x) => x)
            this.switch_data = this.userrole.map((x) => x)
            break
      }
    },
    DataForCreateUpdateUser() {//метод для получения данных для создания нового пользователя. Вводить данные нужно через запятую
      //данные в tempuser берутся из v-text-field
      this.newUser.name = this.tempuser.split(',')[0]//строка => массив и из массива получаются нужные данные
      this.newUser.username = this.tempuser.split(',')[1]
      this.newUser.mail = this.tempuser.split(',')[2]
      this.newUser.password = this.tempuser.split(',')[3]
      this.newUser.updated = this.tempuser.split(',')[4]
      if (this.tempuser.split(',')[5] == 'True' || this.tempuser.split(',')[5] == 'true'){
        this.newUser.is_active = true
      }
      else{
        this.newUser.is_active = false
      }
      this.newUser.created = this.tempuser.split(',')[6]
      this.newUser.role_id = parseInt(this.tempuser.split(',')[7])
      this.newUser.id = parseInt(this.tempuser.split(',')[8])
      
    },
    DataForCreateUpdateRole(){//получение данных для новой роли
    //данные в temprole берутся из v-text-field
      this.newRole.name = this.temprole.split(',')[0]//строка => массив и из массива получаются нужные данные
      this.newRole.id = this.temprole.split(',')[1]
      console.log(this.newRole.name)
    },
    fetchPost(url,data){//POST метод
      fetch(url, {
        method: 'post',
        headers: {
          'Accept': 'application/json, text/plain, */*',
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(data)
      }).then(res => res.json())
        .then(res => console.log(res));
    },
    fetchGetUsers(){//получение данных о пользователях 
      fetch("http://127.0.0.1:8000/users")
            .then(response => response.json())
            .then(json => {
              this.users = json
            })
    },
    fetchGetRoles(){//получение данных о ролях
      fetch("http://127.0.0.1:8000/roles")
            .then(response => response.json())
            .then(json => {
              this.roles = json
            })
    },
    fetchGetUserRoles(){//получение данных о userrole
      fetch("http://127.0.0.1:8000/userroles")
      .then(response => response.json())
      .then(json => {
        this.userrole = json
      })
    },
    fetchPut(url,data){//Put метод
      fetch(url, {
        method: 'put',
        headers: {
          'Accept': 'application/json, text/plain, */*',
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(data)
      }).then(res => res.json())
    },
    fetchDelete(url){
      fetch(url, {
        method: 'delete',
      })
      .then(res => res.json())
    },
    selectActionUser(action){//выбор действия для данных о пользователе
          switch (action) {
            case 'Create'://добавить пользователя
              this.DataForCreateUpdateUser()
              this.fetchPost("http://127.0.0.1:8000/users",this.newUser)
              break
            case 'Update'://обновить пользователя
              this.DataForCreateUpdateUser()
              this.fetchPut(`http://127.0.0.1:8000/users/${this.newUser.id}`,this.newUser)
              break
            case 'Delete'://удалить пользователя
              this.fetchDelete(`http://127.0.0.1:8000/users/${this.iddeleteuserrole}`)
              break
      }
    },
    selectActionRole(action){//выбор действия для данных о роли
          switch (action) {
            case 'Create':
              this.DataForCreateUpdateRole()
              this.fetchPost("http://127.0.0.1:8000/roles",this.newRole)
              break
            case 'Update':
              this.DataForCreateUpdateRole()
              this.fetchPut(`http://127.0.0.1:8000/roles/${this.newRole.id}`,this.newRole)
              break
            case 'Delete':
              this.fetchDelete(`http://127.0.0.1:8000/roles/${this.iddeleterole}`)
              break      
          }
    },
    selectActionUserRole(action){//выбор действия для данных о userrole
    //только создать и удалить т.к. userrole составляется из уже существующих компонентов
      switch (action) {
            case 'Create':
              this.fetchPost(`http://127.0.0.1:8000/userroles/${this.tempuserrole}`)
              break
            case 'Delete':
              this.fetchDelete(`http://127.0.0.1:8000/userroles/${this.iddeleteuserrole}`)
              break
      }
    }       

  }
};
</script>
<style scoped>
  .v-text-field{
      width: 400px;
  } /*изменение ширины поля ввода*/ 
</style>