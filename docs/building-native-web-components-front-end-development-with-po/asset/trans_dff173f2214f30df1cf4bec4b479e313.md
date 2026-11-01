
通过此操作，我们正在为登录和注册用户添加一个表单。我们对两个操作使用相同的视图，并使用 actionText、actionButton 和 linkButton 来切换用户在视图中可以看到的文本，如图 10-4 所示。

![](img/e276a43da8123ca0974f10d5c85948b9_208_0.png)

图 10-4. 登录组件

下一步是添加身份验证功能（清单 10-2）。

## 清单 10-2. 在 Login.vue 中添加 Firebase 身份验证

```
<!-- eslint-disable vue/no-deprecated-slot-attribute -->
<template>
    <div class="formContainer">
        <div class="formLogin">
            <div class="formFields">
                <h1>{{actionText}}</h1>
                <mwc-textfield
                    id="text-email"
                    outlined
                    minlength="3"
label="Email"
    required>
    </mwc-textfield>
</div>
<div class="formFields">
    <mwc-textfield
        id="text-password"
        type="password"
        outlined
        minlength="3"
        label="Password"
        required>
    </mwc-textfield>
</div>
<div class="actionButtons">
    <mwc-button
        id="link-action-button"
        class="actionButton"
        slot="primaryAction"
        @click="goToLink">
        {{linkButton}}
    </mwc-button>
</div>
<div class="actionButtons">
    <mwc-button
        raised
        id="primary-action-button"
        class="actionButton"
        slot="primaryAction"
        @click="submitAction">
        {{actionButton}}
    </mwc-button>
    <mwc-button
        raised
        slot="secondary-action-button"
class="actionButton"
@click="handleClear">
Clear
</mwc-button>
</div>
</div>
</div>
</template>
<script>
import '@material/mwc-textfield';
import '@material/mwc-button';
import { fireApp } from '../firebase';
const auth = fireApp.auth();

export default {
  name: 'Login',
  data() {
    return {
      actionText: 'Login',
      actionButton: 'Send',
      linkButton: 'Register'
    }
  },
  methods: {
    goToLink () {
      switch(this.linkButton) {
      case 'Login':
        this.actionText= 'Login';
        this.actionButton= 'Send';
        this.linkButton = 'Register';
        break;
      case 'Register':
        this.actionText= 'Register';
        this.actionButton= 'Register';
        this.linkButton = 'Login';
break;
}
},
goToDashboard () {
    this.$router.push('/dashboard');
},
signInUser (email, password) {
    auth.signInWithEmailAndPassword(email,password)
    .then(
        () => {
        this.goToDashboard();
        }
    )
    .catch(
        // eslint-disable-next-line
        (error) => {console.log('Something happened.', error)}
    );
},
signUpUser (email, password) {
    auth.createUserWithEmailAndPassword(email,password)
    .then(
        // eslint-disable-next-line
        (user) => {console.log('User registered.', user)}
    )
    .catch(
        // eslint-disable-next-line
        (error) => {console.log('Something happened.', error)}
    );
},
submitAction () {

    let txtEmail = this.$el.querySelector('#text-email');
    let txtPassword = this.$el.querySelector('#text-password');
    const isValid = txtEmail.checkValidity() && txtPassword.checkValidity();
if(isValid) {
    switch(this.actionText) {
        case 'Login':
            this.signInUser (txtEmail.value, txtPassword.value);
            txtEmail.value = '';
            txtPassword.value ='';
            break;
        case 'Register':
            this.signUpUser (txtEmail.value, txtPassword.value);
            txtEmail.value = '';
            txtPassword.value ='';
            break;
    }
}
}
},
</script>
<style scoped>
.formContainer {
    display:flex;
}
.formLogin {
    margin:auto;
}
.formFields {
    margin: 15px;
}
.actionButtons {
    text-align:center;
}
.actionButton {
    margin: 10px;
}
</style>
```

在这里，我们使用 submitAction() 方法来检查我们是在创建新用户还是在应用中登录，并传递用户在表单中输入的电子邮件和密码。如果用户想要登录应用，我们使用 Firebase 的 auth.signInWithEmailAndPassword() 方法来检查用户是否在 Firebase 中存在。如果密码正确，我们将被重定向到 Dashboard 视图。如果用户正在创建新账户，我们使用 Firebase 的 auth.createUserWithEmailAndPassword() 方法将此新用户添加到 Firebase。

最后，我们将修改文件 router/index.js 以添加此新组件（清单 10-3）。

## 清单 10-3. 在 Vue Router 中添加登录组件

```
<!-- eslint-disable vue/no-deprecated-slot-attribute -->
import { createRouter, createWebHistory } from 'vue-router'
import Login from '../views/Login.vue'

const routes = [
  {
    path: '',
    component: Login
  },
  {
    path: '/login',
    name: 'Login',
    component: Login
  },
  {
    path: '/dashboard',
    name: 'Dashboard',
    component: () => import(/* webpackChunkName: "dashboard" */ '../views/Dashboard.vue')
  },
  {
    path: '/about',
    name: 'About',
    component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')
  }
]

const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes
})
export default router
```

从 GitHub 仓库（https://github.com/carlosrojaso/apress-book-web-components）可以访问相关代码，执行 $git checkout v1.0.8。

## 添加守卫

现在，在 App.vue 中，我们必须添加一种机制，防止未登录的用户直接通过 URL 访问 Dashboard.vue。为此，我们将使用 Vue Router 的一个功能，称为 Guards。Guards 主要用于保护导航，通过重定向或取消它们。您可以在 https://router.vuejs.org/guide/advanced/navigation-guards.html#global-before-guards 了解更多关于 Guards 的信息。我们将在 router/index.js 文件中添加此机制（清单 10-4）。

## 清单 10-4. 在 Vue Router 中添加导航守卫

```
import { createRouter, createWebHistory } from 'vue-router'
import Login from '../views/Login.vue'
import { fireApp } from '../firebase';

const routes = [
  {
    path: '',
    component: Login
  },
  {
    path: '/login',
    name: 'Login',
    component: Login
  },
  {
    path: '/dashboard',
    name: 'Dashboard',
    component: () => import(/* webpackChunkName: "dashboard" */ '../views/Dashboard.vue'),
    meta: {
        requiresAuth: true
    }
  },
  {
    path: '/about',
    name: 'About',
    component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')
  }
]

const router = createRouter({
    history: createWebHistory(process.env.BASE_URL),
    routes
})

router.beforeEach(async (to, from, next) => {
    const requiresAuth = to.matched.some(record => record.meta.requiresAuth);
    if (requiresAuth && !await fireApp.getCurrentUser()) {
        next('Login');
    } else {
        next();
    }
})

export default router
```

在这里，我们在 /Dashboard 中使用路由 meta 字段来强制此路由的身份验证，并在守卫中检查它。此外，我们正在检查来自 fireApp 的 getCurrentUser() 方法。我们必须在 firebase.js 中添加此方法（清单 10-5）。

## 清单 10-5. 在 firebase.js 中添加 getCurrentUser() 方法

```
import Firebase from 'firebase';

let config = {
  apiKey: "xxx",
  authDomain: "xxx",
  databaseURL: "xxx",
  projectId: "xxx",
  storageBucket: "xxx",
  messagingSenderId: "xxx",
  appId: "xxx"
};

Firebase.initializeApp(config)

Firebase.getCurrentUser = () => {
  return new Promise((resolve, reject) => {
    const unsubscribe = Firebase.auth().onAuthStateChanged(user => {
      unsubscribe();
      resolve(user);
    }, reject);
  })
};
export const fireApp = Firebase;
```

getCurrentUser 方法为我们提供有关当前用户的信息或拒绝。最后，我们将在顶部添加一个注销链接，以便用户关闭其会话。

在 App.vue 中，我们将在 <mwc-top-app-bar> 中添加一个链接（清单 10-6）。
