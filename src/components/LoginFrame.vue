<template>
    <div v-bind:id="loginFrame" v-if="loginFrame !== 'none'">
        <div v-if="loginFrame !== 'none'" style="position: absolute; right: 10px; top: 4px;"><a href="#" v-on:click="closeFrame"><b-icon icon ="x"  font-scale="1" variant="info"></b-icon></a></div>
        <div id="loginGroup" style="width: 80% ; display: inline-block; text-align: left; z-index: 999" >
            <b-form @submit="login" v-if="loginFrame === 'login'">
                <div style="text-align: center; margin: 20px 0 0 0;" >
                    <label style="font-weight: bold; font-size: 24px">Login</label>
                </div>
                <label style="text-align: left; font-weight: bold">User Name:</label>
                <b-form-input
                        id="input-username"
                        v-model="loginData.name"
                        aria-describedby="input-username-feedback"
                        placeholder="Enter your Username"
                        trim
                ></b-form-input>
                <label style="text-align: left; font-weight: bold">Password:</label>
                <b-form-input
                        id="input-password"
                        v-model="loginData.password"
                        aria-describedby="input-password-feedback"
                        placeholder="Enter your Password"
                        type="password"
                        trim
                ></b-form-input>

                <b-form-group style="margin: 46px 0 0 0">
                    <b-button type="submit" variant="primary" style="width: 100%; font-weight: bold;font-size: larger">Log in</b-button>
                    <div>
                        <br/>
                        <a href="#" v-on:click="toRegisterFrame" style="left: 30px; font-size: small; display: inline-block; float: right">no account, register now ></a>
                    </div>
                </b-form-group>
            </b-form>
            <b-form @submit="register" v-if="loginFrame === 'register'" >
                <div style="text-align: center; margin: 20px 0 0 0;" >
                    <label style="font-weight: bold; font-size: 24px">Register</label>
                </div>
                <label style="text-align: left; font-weight: bold">User Name:</label>
                <b-form-input
                        id="input-username"
                        v-model="registerData.userName"
                        :state="nameState"
                        aria-describedby="input-username-help"
                        placeholder="Enter your Username"
                        trim
                ></b-form-input>
                <b-form-text id="input-username-help" style="letter-spacing:0; font-weight: 200; line-height: 16px; font-size: xx-small;color: #161626">
                    · username can be letters and numbers in 4-12 characters
                </b-form-text>

                <b-row >
                    <b-col sm="6">
                        <label style="text-align: left; font-weight: bold">Password:</label>
                        <b-form-input
                                id="input-password"
                                v-model="registerData.password"
                                :state="passwordState"
                                aria-describedby="input-password-help"
                                placeholder="Enter your Password"
                                type="password"
                                trim
                        ></b-form-input>
                        <b-form-text id="input-password-help" style="letter-spacing:0; font-weight: 200; line-height: 16px; font-size: xx-small;color: #161626">
                            · password length should be 8~16 <br/>
                            · must contain letters (upper or lower case)<br/>
                            · must contain numbers <br/>
                            · must contain special characters <br/>
                        </b-form-text>
                    </b-col>
                    <b-col sm="6">
                        <label style="text-align: left; font-weight: bold">Confirm Password:</label>
                        <b-form-input
                                id="input-confirmPassword"
                                :state="passwordConfirmState"
                                v-model = "registerData.confirmPassword"
                                aria-describedby="input-confirmPassword-feedback"
                                placeholder="Confirm your Password"
                                type="password"
                                trim
                        ></b-form-input>
                        <!-- This will only be shown if the preceding input has an invalid state -->
                        <b-form-invalid-feedback id="input-confirmPassword-feedback" style="text-align: left">
                            Passwords are inconsistent
                        </b-form-invalid-feedback>
                    </b-col>
                </b-row>
                <br/>
                <b-form-group style = "margin : 0 0 0 0">
                    <b-button type="submit" variant="primary"
                              :disabled="registerAvailable"
                              style="width: 100%; font-weight: bold;font-size: larger">Register</b-button>
                    <div >
                        <a href="#" v-on:click="toLoginFrame" style=" font-size: small; display: inline-block; float: right">have account, login now ></a>
                    </div>
                </b-form-group>


            </b-form>

        </div>

    </div>
</template>

<script>
    import Config from "../assets/js/Config";

    export default {
        name: "LoginFrame",
        data() {
            return {
                loginFrame : 'none',
                loginData : {
                    name : '',
                    password : '',
                },
                loginState : {
                    name : true,
                    password: true,
                },
                registerData : {
                    userName : '',
                    password : '',
                    confirmPassword : '',
                },
                registerState : {
                    username : false,
                    password : false,
                    confirmPassword : false,
                }
                ,
                registerAvailable : true,
            }
        },
        methods : {
            toRegisterFrame : function() {
                this.loginFrame = 'none';
                let _this = this;
                this.$parent.$refs.content.sleep(200).then(() => {
                    _this.loginFrame = 'register';
                })
            } ,
            login : function(evt){
                evt.preventDefault();
                let loginData = this.loginData;
                if (loginData.password.trim() ==='' || loginData.name.trim() === '') {
                    alert('Please enter account and password');
                    return;
                }
                //login
                this.$http
                    .get(Config.baseUrl() + 'plant-data/user/login?userName=' + loginData.name + '&password=' + loginData.password)
                    .then(response => {
                        let userInfo = response.data.detail;
                        if (null === userInfo) {
                            alert('Unknown user or wrong password');
                        } else {
                            this.$parent.$refs.top.user.userName = userInfo.userName;
                            this.loginFrame = 'none';
                        }
                    })
            },
            register : function (evt) {
                evt.preventDefault();
                //register
                this.$http
                    .post(Config.baseUrl() + 'plant-data/user/register',this.registerData)
                    .then(response => {
                        let userInfo = response.data.detail;
                        if (null === userInfo) {
                            alert('user already existed');
                        } else {
                            this.$parent.$refs.top.user.userName = userInfo.userName;
                            this.loginFrame = 'none';
                        }
                    }, response => {
                        console.log(response);
                    })
            }
            ,
            toLoginFrame :function () {
                this.loginFrame = 'none';
                let _this = this;
                this.$parent.$refs.content.sleep(200).then(() => {
                    _this.loginFrame = 'login';
                })
            },
            passwordValidate : function () {
                let pattern;
                pattern = /^.*(?=.{8,16})(?=.*\d)(?=.*[a-zA-Z]+)(?=.*[!@#$%^&*?()]).*$/;
                let password = this.registerData.password;
                let state = pattern.test(password) && password.length <= 16;
                if (state) {
                    let registerState = this.registerState;
                    registerState.password = state;
                    this.registerAvailable = !(registerState.password && registerState.username && registerState.confirmPassword);
                } else {
                    this.registerAvailable = true;
                }
                return state;
            },
            usernameValidate : function () {
                let pattern;
                pattern =/^[a-zA-z\d]{4,12}$/;
                let state = pattern.test(this.registerData.userName);
                if (state) {
                    let registerState = this.registerState;
                    registerState.username = state;
                    this.registerAvailable = !(registerState.password && registerState.username && registerState.confirmPassword);
                } else {
                    this.registerAvailable = true;
                }
                return state;
            },
            passwordConfirm : function () {
                let state = this.registerData.password === this.registerData.confirmPassword;
                if (state) {
                    let registerState = this.registerState;
                    registerState.confirmPassword = state;
                    this.registerAvailable = !(registerState.password
                        && registerState.username
                        && registerState.confirmPassword);
                } else {
                    this.registerAvailable = true;
                }
                return state;
            },
            closeFrame : function () {
                this.loginFrame = 'none';
            }
        },
        computed : {
            nameState : function () {
                if (this.registerData.userName === '') {
                    return ;
                }
                return this.usernameValidate();
            },
            passwordState : function () {
                if(this.registerData.password === '') {
                    return ;
                }
                return this.passwordValidate();
            },
            passwordConfirmState : function () {
                if (this.registerData.password === '' || this.registerData.confirmPassword === '') {
                    return;
                }
               return this.passwordConfirm();
            }
        }
    }
</script>

<style scoped>
#login {
    width: 350px;
    height: 345px;
    background-color: rgba(255,255,255,0.99);
    position: fixed;
    top: 50%;
    left: 50%;
    margin: -176px 0 0 -173px;
    box-shadow: 0 1px 4px #AAA;
    border-radius: 10px;
    text-align: center;
}
#register {
    width: 600px;
    height: 400px;
    background-color: rgba(255,255,255,0.99);
    position: fixed;
    top: 50%;
    left: 50%;
    margin: -201px 0 0 -301px;
    box-shadow: 0 1px 4px #AAA;
    border-radius: 10px;
    text-align: center;
}

@-webkit-keyframes fadeIn {
    0% {
        opacity: 0.1; /*初始状态 透明度为0*/
    }
    100% {
        opacity: 1; /*结尾状态 透明度为0*/
    }
}

#register, #login {
    -webkit-animation: fadeIn .4s  linear;
    background-color: #c0c0c8;
    color: #161626;
}
#login ,#register >>> a:link,a:hover,a:visited,a:active {
    color:  #f2f2f4;
}
#login ,#register >>> span {
    color: #161626;
}

</style>