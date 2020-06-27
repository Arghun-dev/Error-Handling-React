# Error-Handling-React

You always get a MsgId and MsgText when you call some action.

Now you want to handle errors, when something goes wrong yoou want to show the error message to the user, but this is a little complicated, because you cannot use the MsgId and MsgText directly in your Alert component, because these error MsgId and MsgText do not change during rendering. So you always have to setState the MsgId and MsgText which you get back from backend then use them in yopur application.

To do that always rememeber you have to use **componentWillReceiveProps** not **componentDidUpdate**

for example in the below code, I receive a MsgId and MsgText from backend and I want to show an alert when the MsgId is equal to -1.

**1. As you can see I've used componentWillReceiveProps life cycle**

**2. Then I have written an if statement to render Alert Message conditionally**

**3. As you can see in my if statement I have noticed if the MsgId is equal to -1 then setState the variables to show the Alert**

**4. And always remember to give the setTimeOut a number for example 12000 milliseconds**

**5. And always remember that you have to import the Alert component in every component which you want to show. not in the parent component like dashboard, because if you import it in the parent component like dashboard, if you change the page between child components inside parent (Dashboard) component, in this case the Alert component will be re-rendered and if you change the page you will see the same alert shows in that page without doing anything, so this is wrong.**
Example:

```
constructor(props) {
    super(props);
    
    this.state = {
        showAlert: false,
        alertStatus: null,
        alertText: null,
        dashboardAlertHeading: null
    }
}

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
        const { modal, modalHeader, id, height, width } = this.state;
        return (
            <div className='animated fadeIn'>
                <span style={{ position: 'absolute', width: '70%', top: 0, left: 'auto', right: 'auto' }}>
                    <DashboardAlert
                        showAlert={this.state.showAlert}
                        alertStatus={this.state.alertStatus}
                        alertText={this.state.alertText}
                        dashboardAlertHeading={this.state.dashboardAlertHeading}
                        resetDashboardAlert={this.resetDashboardAlert}
                    />
                </span>
                <Form1
                    Token={this.state.Token}
                    cardBody={this.renderCardBody()}
                    closeModal={this.closeModal}
                    deleteProductCategory={this.deleteProductCategory}
                    modal={modal}
                    modalHeader={modalHeader}
                    id={id}
                    height={height}
                    width={width}
                    ProductCategoryID={this.state.ProductCategoryID}
                    rowguid={this.state.rowguid}
                    ProductCategoryCode={this.state.ProductCategoryCode}
                    ProductCategoryName={this.state.ProductCategoryName}
                    ProductCategoryID={this.state.ProductCategoryID}
                />
            </div>
        )
    }
}

const mapStateToProps = state => {
    return {
        loading: state.productCategory.loading,
        token: state.auth[0].token,
        productCategory: state.productCategory.productCategory,
        productCategoryMsgId: state.productCategory
    }
}

export default connect(mapStateToProps, { getProductCategory, productCategory_Delete })(ProductCategory);
```
