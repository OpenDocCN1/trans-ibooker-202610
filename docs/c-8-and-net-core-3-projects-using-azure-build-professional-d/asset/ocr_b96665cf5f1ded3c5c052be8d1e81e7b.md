```
get => _username;
set
{
    UpdateField(ref _username, value);
}
}

private string _password;

public string Password
{
    get => _password;
    set
    {
        UpdateField(ref _password, value);
    }
}
```

The last two things we need are the LoginCommand wiring up and the data context setting; let's start with LoginCommand:

```
public LoginViewModel()
{
    LoginCommand = new RelayCommand(() =>
    {
        DoLogin();
    });
}
```

```
private void DoLogin()
{
    throw new NotImplementedException();
}
```

```
public RelayCommand LoginCommand { get; set; }
```

Let's set the data context. Then, we should be able to see the login screen do everything bar actually logging in (inside LoginView.xaml.cs):

```
public LoginView()
{
    this.InitializeComponent();
    ViewModel = new LoginViewModel();
    DataContext = ViewModel;
}
```

```
public LoginViewModel ViewModel { get; set; }
```