```
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
}
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