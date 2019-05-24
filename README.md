# Simple Form Mailbox

This project is developped width Vue.js, It helps to read the emails send by SimpleForm

## Project setup
```
yarn install
```

### Create .env file

```
.env                # loaded in all cases
.env.local          # loaded in all cases, ignored by git
```
and add your simpleform_token
```
VUE_APP_SIMPLEFORM_TOKEN=<api_token>
```

### Compiles and hot-reloads for development
```
yarn run serve
```

### Compiles and minifies for production
```
yarn run build
```