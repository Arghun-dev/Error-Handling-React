# Error-Handling-React

You always get a MsgId and MsgText when you call some action.

Now you want to handle errors, when something goes wrong yoou want to show the error message to the user, but this is a little complicated, because you cannot use the MsgId and MsgText directly in your Alert component, because these error MsgId and MsgText do not change during rendering. So you always have to setState the MsgId and MsgText which you get back from backend then use them in yopur application.

To do that always rememeber you have to use **componentWillReceiveProps** not **componentDidUpdate**

for example in the below code, I receive a MsgId and MsgText from backend and I want to show an alert when the MsgId is equal to -1.

**1. As you can see I've used componentWillReceiveProps life cycle**

**2. Then I have written an if statement to render Alert Message conditionally**

**3. As you can see in my if statement I have noticed if the MsgId is equal to -1 then setState the variables to show the Alert**

**4. And always remember to give the setTimeOut a number for example 12000 milliseconds**

Example:

```
resetDashboardAlert() {
    this.setState({
      showAlert: false,
      alertStatus: null,
    })
}
 
componentWillReceiveProps(nextProps) {
    if (nextProps.productCategoryMsgId[0]?.MsgId === -1) {
      this.setState({
        showAlert: true,
        alertStatus: 'danger',
        alertText: nextProps.productCategoryMsgId[0].MsgText,
        dashboardAlertHeading: 'عملیات ناموفق'
      })
    }

    setTimeout(() => {
      this.setState({
        showAlert: false,
        alertStatus: null,
        alertText: null,
        dashboardAlertHeading: null
      })
    }, 12000)
}

render() {
    const { token } = this.props;
    if (token) {
      return (
        <BrowserRouter>
          <div className="App">
            <DashboardAlert
              resetDashboardAlert={this.resetDashboardAlert}
              showAlert={this.state.showAlert}
              alertStatus={this.state.alertStatus}
              alertText={this.state.alertText}
              dashboardAlertHeading={this.state.dashboardAlertHeading}
            />
            <React.Suspense fallback={loading()}>
              <Switch>
                <Route
                  exact
                  path="/Profile"
                  name="Profile"
                  render={(props) => <Profile {...props} />}
                />
                <Route
                  path="/"
                  name="Home"
                  render={(props) => <DefaultLayout changeDashboardAlertParams={this.changeDashboardAlertParams} {...props} />}
                />
              </Switch>
            </React.Suspense>
          </div>
        </BrowserRouter>
      );
    } else return <Redirect to='/' />
  }
}

const mapStateToProps = state => {
  if (state.auth[0]) {
    return {
      token: state.auth[0].token,
      addUserMsgId: state.users,
      groupsMsgId: state.groups,
      productCategoryMsgId: state.productCategory
    }
  } else return {}
}

export default connect(mapStateToProps, { userInfoGet, userProfileImage_Get })(App);
```
