<template>
  <div>
    <v-card color="black">
      <v-row justify="" align="" id="charter">
        <v-col cols="12" sm="12" md="12">
          <v-toolbar color="black" elevation="0" extended extension-height="30">
            <v-spacer />
            <nuxt-link class="nuxt-link" to="/">
              <v-img
                style="margin-top: 38px; margin-start: 36px"
                :src="logo"
                contain
                height="90"
              >
              </v-img>
            </nuxt-link>
            <v-spacer />
            <div style="margin-top: 40px">
              <v-chip text small @click="logout">
                <v-icon small color="red">mdi-logout</v-icon>
              </v-chip>
            </div>
          </v-toolbar>
        </v-col>

        <v-col cols="12" sm="12" md="12">
          <v-img :src="payment2" contain height="200" />
        </v-col>

        <v-col cols="12" sm="12" md="12"> </v-col>

        <v-col cols="12" sm="12" md="12">
          <div class="container">
            <v-tabs background-color="black" centered dark>
              <v-tab
                @click="
                  (pin_status = ''),
                    (pin_input = ''),
                    (amount_input = ''),
                    (deposit = true)
                "
              >
                <v-icon color="green">mdi-cash-refund</v-icon> Power bank Sale
              </v-tab>
              <v-tab
                @click="
                  (deposit = false),
                    (pin_status = ''),
                    (pin_input = ''),
                    (amount_input = '')
                "
              >
                <v-icon color="green">mdi-arrow-up-circle</v-icon>Deposit
              </v-tab>
            </v-tabs>
          </div>
        </v-col>

        <v-col v-show="!deposit" cols="12" sm="12" md="12">
          <div class="d-flex" id="charter">
            <v-spacer />
            <div>
              <div class="d-flex">
                <div class="container">
                  <v-form ref="form" v-model="valid" lazy-validation>
                    <v-text-field
                      clearable
                      v-model="depo_amount"
                      outlined
                      dense
                      rounded
                      :rules="nameRules"
                      type="number"
                      label="Enter amount"
                      required
                    ></v-text-field>

                    <v-text-field
                      clearable
                      v-model="depo_number"
                      outlined
                      dense
                      rounded
                      type="number"
                      :rules="nameRules"
                      label="Provide Mpesa number"
                      placeholder="7-34-340-774"
                      required
                    ></v-text-field>

                    <!--  -->
                    <v-btn
                      color="green"
                      block
                      class="mr-4 text-caption text--black"
                      @click="MpesaPaymentStk()"
                    >
                      Make a Deposit
                    </v-btn>
                    <div class="container">
                      <div class="text--green" v-show="snackbar" style="color: green">
                        {{ successResponse }}
                      </div>
                      <div class="text--green" v-show="snackbar2" style="color: red">
                        {{ errorResponse }}
                      </div>
                    </div>
                  </v-form>
                </div>

                <v-spacer></v-spacer>
              </div>
            </div>

            <v-spacer />
          </div>
        </v-col>
        <v-col v-show="deposit" cols="12" sm="12" md="12">
          <div class="d-flex" id="charter">
            <v-spacer />

            <div class="text-center">
              <div class="">
                <div class="d-flex text-center">
                  <v-form>
                    <div class="">
                      <v-row>
                        <v-col cols="12" sm="12" md="12">
                          <div>
                            <v-col cols="12" sm="12" md="12">
                              <div class="d-flex">
                                <div class="col-md-12">
                                  <v-form ref="form" v-model="valid" lazy-validation>
                                    <v-text-field
                                      clearable
                                      v-model="Quantity"
                                      outlined
                                      dense
                                      @change="getTotalAmount()"
                                      :rules="nameRules"
                                      type="number"
                                      label="Quantity"
                                      required
                                    ></v-text-field>

                                    <v-text-field
                                      clearable
                                      v-model="Amount"
                                      outlined
                                      dense
                                      :rules="nameRules"
                                      type="number"
                                      label="Enter amount"
                                      required
                                    ></v-text-field>

                                    <v-text-field
                                      clearable
                                      v-model="Phone"
                                      outlined
                                      dense
                                      type="number"
                                      :rules="nameRules"
                                      label="Provide Mpesa number"
                                      placeholder="7-34-340-774"
                                      required
                                    ></v-text-field>

                                    <!--  -->
                                    <v-btn
                                      color="green"
                                      block
                                      class="mr-4 text-caption"
                                      @click="MpesaPaymentStk2()"
                                    >
                                      Request Stk
                                    </v-btn>

                                    <div class="container">
                                      <div
                                        class="text--green"
                                        v-show="snackbar"
                                        style="color: green"
                                      >
                                        {{ successResponse }}
                                      </div>
                                      <div
                                        class="text--green"
                                        v-show="snackbar2"
                                        style="color: red"
                                      >
                                        {{ errorResponse }}
                                      </div>
                                    </div>
                                  </v-form>
                                </div>

                                <v-spacer></v-spacer>
                              </div>
                            </v-col>
                          </div>
                        </v-col>
                      </v-row>
                    </div>
                  </v-form>
                </div>
              </div>
            </div>

            <v-spacer />
          </div>
        </v-col>
        <div class="">
          <v-progress-linear
            v-show="show6"
            color="green accent-4"
            indeterminate
            rounded
            height="6"
          ></v-progress-linear>
        </div>
      </v-row>

      <v-snackbar
        color="primary accent-8"
        :timeout="6000"
        v-model="snackbar_s"
        centered
        bottom
      >
        {{ snackbarText_s }}
      </v-snackbar>
      <v-snackbar
        light
        color="success text--black"
        :timeout="3000"
        v-model="snackbar"
        center
      >
        {{ snackbarText }}
      </v-snackbar>
      <v-snackbar color="error" :timeout="2500" v-model="snackbar2" outlined center>
        {{ snackbarText2 }}
      </v-snackbar>
    </v-card>
    <v-dialog v-model="pin_dialog2">
      <div class="d-flex">
        <v-spacer />
        <v-card>
          <div class="d-flex">
            <v-spacer />
            <v-btn icon @click="(pin_dialog2 = false), (pin_transact = null)">
              <v-icon color="red">mdi-close</v-icon>
            </v-btn>
            <v-spacer />
          </div>
          <div class="container">
            <v-card-subtitle>Enter pin</v-card-subtitle>
            <div class="text-center">
              <div class="">
                <div class="d-flex text-center">
                  <v-form>
                    <v-text-field
                      width="200"
                      clearable
                      :rules="rules"
                      outlined
                      v-model="pin_transact"
                      :counter="4"
                      type="password"
                      label="Enter pin"
                    >
                    </v-text-field>
                    <v-btn
                      color="green"
                      class="black--text"
                      @click="verifyPin22(encrypteData(pin_transact))"
                      >Verify pin</v-btn
                    >
                    <v-btn :to="`/resetpin/${uid}`" text> Forgot pin</v-btn>
                  </v-form>
                </div>
              </div>
            </div>
            <v-card-actions> </v-card-actions>
          </div>
        </v-card>
        <v-spacer />
      </div>
    </v-dialog>
    <v-dialog v-model="pin_dialog">
      <div class="d-flex">
        <v-spacer />
        <v-card>
          <div class="container">
            <div class="d-flex">
              <v-spacer />
              <v-btn icon @click="(pin_dialog = false), (pin_transfer = null)">
                <v-icon color="red">mdi-close</v-icon>
              </v-btn>
              <v-spacer />
            </div>
            <v-card-subtitle>Enter pin</v-card-subtitle>
            <div class="text-center">
              <div class="">
                <div class="d-flex text-center">
                  <v-form>
                    <v-text-field
                      width="200"
                      clearable
                      outlined
                      :rules="rules"
                      v-model="pin_transfer"
                      :counter="8"
                      type="password"
                      label="Enter pin"
                    >
                    </v-text-field>
                    <v-btn
                      color="green"
                      class="black--text"
                      @click="verifyPin2(encrypteData(pin_transfer))"
                      >Verify pin</v-btn
                    >
                    <v-btn :to="`/resetpin/${uid}`" text> Forgot pin</v-btn>
                  </v-form>
                </div>
              </div>
            </div>
            <v-card-actions> </v-card-actions>
          </div>
        </v-card>
        <v-spacer />
      </div>
    </v-dialog>
    <v-dialog v-model="pin_view_bal_dialog">
      <div class="d-flex">
        <v-spacer />
        <v-card>
          <div class="d-flex">
            <v-spacer />
            <v-btn icon @click="pin_view_bal_dialog = false">
              <v-icon color="red">mdi-close</v-icon>
            </v-btn>
            <v-spacer />
          </div>

          <div class="container">
            <v-card-subtitle>Enter pin</v-card-subtitle>
            <div class="text-center">
              <div class="">
                <div class="d-flex text-center">
                  <v-form>
                    <v-text-field
                      width="200"
                      clearable
                      outlined
                      :rules="rules"
                      v-model="pinbal"
                      :counter="4"
                      type="password"
                      label="Enter pin"
                    >
                    </v-text-field>
                    <v-btn
                      color="green"
                      class="black--text"
                      @click="verifyPin24(encrypteData(pinbal))"
                      >Verify pin</v-btn
                    >
                    <v-btn :to="`/resetpin/${uid}`" text> Forgot pin</v-btn>
                  </v-form>
                </div>
              </div>
            </div>
            <v-card-actions> </v-card-actions>
          </div>
        </v-card>
        <v-spacer />
      </div>
    </v-dialog>

    <v-dialog v-model="pin_set_dialog">
      <div class="d-flex">
        <v-spacer />

        <v-card class="" max-width="250">
          <div class="d-flex">
            <v-spacer />
            <v-btn icon @click="pin_set_dialog = false">
              <v-icon color="red">mdi-close</v-icon>
            </v-btn>
            <v-spacer />
          </div>

          <div class="container">
            <div class="text-center">
              <div class="">
                <v-card-title>Set your pin</v-card-title>
                <div class="d-flex text-center">
                  <v-form>
                    <v-col>
                      <div class="">
                        <v-row>
                          <v-col v-show="!code_state1" cols="12" md="12">
                            <div class="container text-center">
                              <div class="container">
                                <div class="text-center">
                                  <v-text-field
                                    width="200"
                                    outlined
                                    :rules="rules"
                                    v-model="pin"
                                    :counter="4"
                                    type="password"
                                    label="Provide pin "
                                  ></v-text-field>
                                  <v-text-field
                                    width="200"
                                    outlined
                                    :rules="rules"
                                    v-model="verify_pin"
                                    :counter="4"
                                    type="password"
                                    @change="
                                      password_status = CheckPassword(
                                        encrypteData(pin),
                                        encrypteData(verify_pin)
                                      )
                                    "
                                    label="Verify pin "
                                  ></v-text-field>
                                  <h5>{{ pass_status }}</h5>
                                </div>
                              </div>

                              <div class="d-flex">
                                <v-spacer />
                                <div
                                  id="recaptcha-container2"
                                  data-sitekey="AIzaSyA0emGzJyErokuL84Eb_Fy4YWRxdXBi6Jo"
                                  data-callback="sendForm"
                                  data-size="visible"
                                ></div>
                                <v-spacer />
                              </div>

                              <v-btn color="black" class="green--text" @click="setPin()"
                                >Set PIN</v-btn
                              >
                            </div>
                          </v-col>
                        </v-row>
                      </div>
                    </v-col>
                  </v-form>
                </div>
              </div>
            </div>
            <v-card-actions> </v-card-actions>
          </div>
        </v-card>

        <v-spacer />
      </div>
    </v-dialog>
  </div>
</template>

<script>
import CryptoJS from "crypto-js";
import axios from "axios";
import dayjs from "@nuxtjs/dayjs";
import moment from "moment";

import { uuid } from "vue-uuid";

const keyValue = "fd85b4945YF'i"; // your key value (eg: key)
const ivKey = "smslt";

export default {
  name: "accounts",
  data() {
    return {
      rules: [
        (value) => !!value || "Required.",
        (value) => (value || "").length <= 4 || "Max 4 characters",
      ],
      deposit: true,
      Quantity: 1,
      depo_amount: "",
      depo_number: "",
      showB: false,
      pin_input: "",
      pin_input_deposit: "",
      cash_deposit_dialog: false,
      cash_refund_dialog: false,
      pin_set_dialog: false,
      pin_dialog: false,
      pin_dialog2: false,
      pin_view_bal_dialog: false,
      pin: null,
      pinbal: null,
      pin_transfer: null,
      pin_transact: null,
      refund: false,
      password_status: false,
      stk_push: false,
      verify_pin: null,
      auth_state: false,
      security_quiz: false,
      security_key: false,
      secretKey: null,
      set_Pin: false,
      pass_status: "",
      pin: null,
      verify_pin: null,
      b2c: false,
      bg: require("@/assets/bg.png"),
      logo: require("@/assets/logo.svg"),
      payment2: require("@/assets/payment.svg"),
      errorResponse: "",
      successResponse: "",
      Amount: 2000,
      Phone: null,
      UserName: "",
      snackbar_s: false,
      snackbarText_s: "",
      snackbar: false,
      snackbar2: false,
      snackbarText: "",
      snackbarText2: "",
      status: false,
      timerEnabled: false,
      show6: false,
      timerCount: 15,
      timer_Count: 15,
      valid: true,
      name: "",
      nameRules: [(v) => !!v || "Name is required"],
      email: "",
      emailRules: [
        (v) => !!v || "E-mail is required",
        (v) => /.+@.+\..+/.test(v) || "E-mail must be valid",
      ],
      select: null,
      items: ["Item 1", "Item 2", "Item 3", "Item 4"],
      checkbox: false,
      CheckoutRequestID: "",
      balance: "0",
      transactions: [],
      show6: false,
      errorResponse: "",
      successResponse: "",
      timerEnabled3: false,
      timerCount3: 2,
      timerEnabled2: false,
      timerCount2: 2,
      uid: null,
      payment: false,
      payment_msg: "",
    };
  },
  methods: {
    getTotalAmount() {
      let totalAmount = this.Quantity * 1000;

      this.Amount = totalAmount;

      return this.Amount;
    },
    CheckPhone() {
      if (this.Phone == "254106150255") {
        this.snackbar2 = true;
        this.snackbarText2 = "This number is not authorized ";
      } else {
        this.pin_dialog = true;
        this.pin_transfer = null;
      }
    },
    checkColor(val) {
      if (val == "Deposit") {
        return "green2";
      } else if (val == "Withdraw") {
        return "red";
      } else if (val == "Tips") {
        return "blue";
      }
    },
    checkType(val) {
      if (val == "Deposit") {
        return "arrow-up";
      } else if (val == "Withdraw") {
        return "arrow-down";
      } else if (val == "Tips") {
        return "hand-coin";
      }
    },

    verifyPin24(val) {
      if (this.decrypteData(this.pin) === this.decrypteData(val)) {
        console.log("PIn match");
        this.FetchBalance();
        this.pin_status = "";
        this.showB = true;
        this.pin_view_bal_dialog = false;
      } else {
        console.log("PIn not match");
        this.pin_status = "Invalid pin. \nProvide a valid pin number";
        this.snackbar2 = true;
        this.snackbarText2 = "Please enter a valid pin number";
      }
    },
    verifyPin22(val) {
      if (this.decrypteData(this.pin) === this.decrypteData(val)) {
        console.log("PIn match");
        this.MpesaPaymentStk();
        this.pin_status = "";
        this.pin_dialog = false;
      } else {
        console.log("PIn not match");
        this.pin_status = "Invalid pin. \nProvide a valid pin number";
        this.snackbar2 = true;
        this.snackbarText2 = "Invalid pin. \nProvide a valid pin number";
      }
    },
    verifyPin2(val) {
      if (this.decrypteData(this.pin) === this.decrypteData(val)) {
        console.log("PIn match");

        this.pin_status = "";
        this.pin_dialog = false;
        this.pin_transfer = null;
      } else {
        console.log("PIn not match");
        this.pin_status = "Invalid pin. \nProvide a valid pin number";
        this.snackbar2 = true;
        this.snackbarText2 = "Invalid pin. \nProvide a valid pin number";
      }
    },
    encrypteData(data) {
      if (this.secretKey == null) {
        this.pass_status = "UnAble detect key";
      } else {
        if (data != null) {
          const key = CryptoJS.PBKDF2(this.secretKey, "salt", {
            keySize: 256 / 32,
            iterations: 1000,
          });
          const iv = CryptoJS.enc.Utf8.parse(ivKey); // Convert string to WordArray
          const encrypted = CryptoJS.AES.encrypt(data, key, {
            iv: iv,
            mode: CryptoJS.mode.CBC,
          });

          this.encryptedText = encrypted.ciphertext.toString(CryptoJS.enc.Hex);
          return encrypted.ciphertext.toString(CryptoJS.enc.Hex);
        }
      }
    },
    decrypteData(data) {
      if (this.secretKey == null) {
        this.pass_status = "UnAble detect key";
      } else {
        if (data) {
          const key = CryptoJS.PBKDF2(this.secretKey, "salt", {
            keySize: 256 / 32,
            iterations: 1000,
          });
          const iv = CryptoJS.enc.Utf8.parse(ivKey);
          const decrypted = CryptoJS.AES.decrypt(
            {
              ciphertext: CryptoJS.enc.Hex.parse(data),
            },
            key,
            {
              iv: iv,
              mode: CryptoJS.mode.CBC,
            }
          );
          return decrypted.toString(CryptoJS.enc.Utf8);
        }
      }
    },
    CheckPassword(val, val2) {
      if (val == val2) {
        this.pass_status = "Password matches";
        return true;
      } else {
        this.pass_status = "Password does not match";
        return false;
      }
    },
    ConfirmCode2() {
      console.log("Code sent to you.", this.code_no2);

      var credential = firebase.auth.PhoneAuthProvider.credential(
        this.confirmation_Result.verificationId,
        this.code_no2
      );
      if ((this.code_no2 = "")) {
        this.snackbar2 = true;
        this.snackbarText2 = "Provide Code sent to you";
        console.log("Provide Code sent to you");
      } else {
        console.log("Verify Code", this.code_no2);
        this.progress_bar = true;
        firebase
          .auth()
          .signInWithCredential(credential)
          .then((user) => {
            // SMS sent. Prompt user to type the code from the message, then sign the
            // user in with confirmationResult.confirm(code).
            //this.$toast.success("Otp sent successfully");
            window.location.reload(true);
            this.FetchUserPin();
          })
          .catch((error) => {
            this.progress_bar = false;
            // Error; SMS not sent
            console.log("Error", error);
            this.snackbar2 = true;
            this.snackbarText2 = error;
          });
      }
    },
    logout() {
      this.$fire.auth.signOut();
      window.location.reload(true);
    },

    validate() {
      this.$refs.form.validate();
    },
    reset() {
      this.$refs.form.reset();
    },
    resetValidation() {
      this.$refs.form.resetValidation();
    },
    MpesaPaymentStk2() {
      let that = this;
      if (that.Phone == null) {
        that.snackbarText2 = "Provide phone number..";
        that.snackbar2 = true;
      } else if (this.Amount == "") {
        that.snackbarText2 = "Provide amount.";
        that.snackbar2 = true;
      } else {
        that.show6 = true;
        axios
          .post("https://chargeb2c-78a6d3d19f7e.herokuapp.com/stk_shop", {
            Phonenumber: "254" + that.Phone,
            amount: that.Amount,
          })
          .then(function (response) {
            console.log(response);
            if (response.status == 200) {
              if (response.data.errorCode == "400.002.02") {
                that.snackbar2 = true;
                that.snackbarText2 = response.data.errorMessage;
              } else if (response.data.errorCode == "500.001.1001") {
                that.snackbar2 = true;
                that.snackbarText2 = response.data.errorMessage;
              } else {
                that.snackbar = true;
                that.snackbarText = response.data.CustomerMessage;
                that.successResponse = response.data.CustomerMessage;
                that.CheckoutRequestID = response.data.CheckoutRequestID;
                console.log(that.CheckoutRequestID);
                that.timerEnabled = true;
              }
            } else if (response.status == 400) {
              that.snackbar2 = true;
              that.snackbarText2 = response.data;
              that.errorMessage = response.data;
            }
          })
          .catch(function (error) {
            console.log(error);
            that.snackbarText2 = error;
            that.snackbar2 = true;
            that.show6 = false;
          });
      }
    },
    MpesaPaymentStk() {
      let that = this;
      if (that.depo_number == null) {
        that.snackbarText2 = "Provide phone number..";
        that.snackbar2 = true;
      } else if (this.depo_amount == "") {
        that.snackbarText2 = "Provide amount.";
        that.snackbar2 = true;
      } else {
        that.show6 = true;
        axios
          .post("https://chargeb2c-78a6d3d19f7e.herokuapp.com/stk_shop", {
            Phonenumber: "254" + that.depo_number,
            amount: that.depo_amount,
          })
          .then(function (response) {
            console.log(response);
            if (response.status == 200) {
              if (response.data.errorCode == "400.002.02") {
                that.snackbar2 = true;
                that.snackbarText2 = response.data.errorMessage;
                that.show6 = false;
                that.pin_transact = null;
              } else if (response.data.errorCode == "500.001.1001") {
                that.snackbar2 = true;
                that.snackbarText2 = response.data.errorMessage;
                that.show6 = false;
                that.pin_transact = null;
              } else {
                that.snackbar = true;
                that.snackbarText = response.data.CustomerMessage;
                that.successResponse = response.data.CustomerMessage;
                that.CheckoutRequestID = response.data.CheckoutRequestID;
                console.log(that.CheckoutRequestID);
                that.timerEnabled = true;
                that.pin_dialog2 = false;
                that.pin_transact = null;
              }
            } else if (response.status == 400) {
              that.snackbar2 = true;
              that.snackbarText2 = response.data;
              that.errorMessage = response.data;
              that.show6 = false;
              that.pin_transact = null;
            }
          })
          .catch(function (error) {
            console.log(error);
            that.snackbarText2 = error;
            that.snackbar2 = true;
            that.show6 = false;
            that.pin_transact = null;
          });
      }
    },
    ////Stk Query////
    StkQuery() {
      let that = this;
      that.snackbar_s = true;
      that.snackbarText_s = "Checking payment status...";
      axios
        .post("https://chargeb2c-78a6d3d19f7e.herokuapp.com/stk_shop/query", {
          checkoutRequestId: that.CheckoutRequestID,
        })
        .then(function (response) {
          console.log("StkPush Query", response.data);

          if (response.status == 200) {
            if (response.data.errorCode == "400.002.02") {
              that.snackbar2 = true;
              that.snackbarText2 = response.data.errorMessage;
              that.errorResponse = response.data.errorMessage;
              that.timerCount = 15;
              that.timerEnabled = false;
            } else if (response.data.errorCode == "500.001.1001") {
              that.snackbar2 = true;
              that.snackbarText2 = response.data.errorMessage;
              that.errorResponse = response.data.errorMessage;
              that.timerCount = 15;
              that.timerEnabled = false;
            } else {
              if (response.data.ResultCode == "0") {
                that.snackbar = true;
                that.snackbarText = response.data.ResultDesc;
                that.successResponse = response.data.ResultDesc;
                that.timerEnabled = false;
                that.timerCount = 15;
              } else if (response.data.ResultCode == "1032") {
                that.snackbar2 = true;
                that.snackbarText2 = "Request was cancelled";
                that.errorResponse = "Request was cancelled";
                that.timerCount = 15;
                that.timerEnabled = false;
              } else if (response.data.ResultCode == "2001") {
                that.snackbar2 = true;
                that.snackbarText2 = "You entered a wrong pin";
                that.errorResponse = "You entered a wrong pin";
                that.timerCount = 15;
                that.timerEnabled = false;
              } else {
                that.snackbar2 = true;
                that.snackbarText2 = response.data.ResultDesc;
                that.errorResponse = response.data.ResultDesc;
                that.timerEnabled = false;
                that.timerCount = 15;
              }
            }
          } else if (response.status == 400) {
            that.snackbar2 = true;
            that.snackbarText2 = response.data;
            that.timerCount = 15;
            that.timerEnabled = false;
          }
        })
        .catch(function (error) {
          that.snackbar2 = true;
          that.snackbarText2 = error;
          that.timerCount = 15;
          that.timerEnabled = false;
          that.show6 = false;
        });
    },
    loginAnonymously1() {
      this.$fire.auth
        .signInAnonymously()
        .catch(function (error) {
          this.snackbarText = error.message;
          this.snackbar = true;
          this.showLogin = false;
        })
        .then((user) => {
          //we are signed in
          const start_time = this.$dayjs(new Date()).format("YYYY/MM/DD HH:mm:ss");
          let ID = uuid.v1();
          console.log(uuid.v1());
          const db = this.$fire.firestore;
          db.collection("Charge24_users")
            .doc(user.user.uid)
            .set({
              user_id: user.user.uid,
              start_time: start_time,
              ref: ID,
              active: true,
              phone_no: this.Phone,
            })
            .then((docRef) => {
              console.log("User logged in");
              this.snackbar = true;
              this.snackbarText = "Process Successfully completed";
              this.$router.push({
                path: "/timer",
              });
            })
            .catch((error) => {
              console.log("Error adding document: ", error);
            });
        });
    },
    uploadDetails(val) {
      const db = this.$fire.firestore;
      const start_timer = this.$dayjs(new Date()).format("YYYY/MM/DD HH:mm:ss");
      let ID = uuid.v1();
      console.log(uuid.v1());
      db.collection("charge24_users")
        .doc(val)
        .set({
          user_id: val,
          start_time: start_timer,
          ref: ID,
          active: true,
        })
        .then(() => {
          console.log("User logged in");
          this.$router.push({
            path: "/timer",
          });
        })
        .catch(function (error) {
          console.log("Error adding document: ", error);
        });
    },
    logout() {
      this.$fire.auth.signOut();
      window.location.reload(true);
    },
    checkUser() {
      if (this.$fire.auth.currentUser != null) {
        this.secretKey = this.$fire.auth.currentUser.uid;
        this.uid = this.$fire.auth.currentUser.uid;
        this.FetchUserPin(this.$fire.auth.currentUser.uid);
        console.log(this.secretKey, "keys");
      } else {
        this.auth_state = false;
        console.log("No user logged in");
      }
    },
    generateRandomNumber() {
      const digits = "DY*1234FA6789";
      let randomNumber = "";
      const length = 9;

      for (let i = 0; i < length; i++) {
        const randomIndex = Math.floor(Math.random() * digits.length);
        randomNumber += digits[randomIndex];
      }

      this.randomNineDigitNumber = randomNumber;

      return randomNumber;
    },
    FetchUserPin(val) {
      const db = this.$fire.firestore;
      var docRef = db.collection("charge24_admin_user").doc(val);
      docRef
        .get()
        .then((doc) => {
          if (doc.exists) {
            this.pin_set_dialog = false;
            this.pin = doc.data().pin;
            this.payment = doc.data().payment;
          } else {
            console.log("pin null");
          }
        })
        .catch((error) => {
          console.log("Error getting document:", error);
        });
    },
    FetchUserPin2() {
      const db = this.$fire.firestore;
      var docRef = db
        .collection("charge24_admin_user")
        .doc(this.$fire.auth.currentUser.uid);
      docRef
        .get()
        .then((doc) => {
          if (doc.exists) {
            this.payment = doc.data().payment;
            this.payment_msg = doc.data().msg;
            if (this.payment == false) {
              this.snackbar2 = true;
              this.snackbarText2 = this.payment_msg;
              this.resetPin2();
            } else if (this.payment == true) {
              this.snackbar = true;
              this.snackbarText = "Refund Successfully completed";
              this.resetPin2();
            }
          } else {
            console.log("pin null");
          }
        })
        .catch((error) => {
          console.log("Error getting document:", error);
        });
    },
    FetchBalance() {
      const db = this.$fire.firestore;
      var docRef = db.collection("Admin").doc("Account");
      docRef
        .get()
        .then((doc) => {
          if (doc.exists) {
            this.balance = doc.data().account_balance;
          } else {
            // doc.data() will be undefined in this case
            console.log("No such document!");
          }
        })
        .catch((error) => {
          console.log("Error getting document:", error);
        });
    },
    checkPin(val) {
      console.log(val);
    },
    setPin() {
      if (this.password_status == false) {
        this.snackbar2 = true;
        this.snackbarText2 = "Password does not match";
      } else {
        this.progress_bar = true;
        const db = this.$fire.firestore;
        const docRef = db
          .collection("charge24_admin_user")
          .doc(this.$fire.auth.currentUser.uid);

        const newData = {
          pin: this.encrypteData(this.verify_pin),
          Account_id: this.encrypteData(this.$fire.auth.currentUser.uid),
          user_uid: this.$fire.auth.currentUser.uid,
          // Add more fields as needed
        };

        db.runTransaction(async (transaction) => {
          transaction.set(docRef, newData);
        })
          .then((docRef) => {
            console.log("Pin set Successfully ");
            this.snackbar = true;
            this.snackbarText = "Pin set Successfully ";
            this.progress_bar = false;
            this.FetchUserPin();
          })
          .catch((error) => {
            this.snackbar2 = true;
            this.snackbarText2 = error;
            this.progress_bar = false;
            console.error("Error adding listing: ", error);
          });
      }
    },
    resetPin2() {
      const db = this.$fire.firestore;
      console.log("Reset pin", uuid.v1());
      db.collection("charge24_admin_user")
        .doc(this.$fire.auth.currentUser.uid)
        .update({
          payment: false,
        })
        .then(() => {
          that.show6 = false;
          this.timerCount2 = 2;
          this.timerCount3 = 2;
        })
        .catch(function (error) {
          console.log("Error adding document: ", error);
        });
    },
  },
  mounted() {},
  created() {
    this.checkUser();
    this.FetchBalance();
  },
  watch: {
    timerEnabled(value) {
      if (value) {
        setTimeout(() => {
          this.timerCount--;
        }, 1000);
      }
    },
    timerCount: {
      handler(value) {
        if (value > 0 && this.timerEnabled) {
          setTimeout(() => {
            this.timerCount--;
          }, 1000);
        } else if (value == 0) {
          console.log("Done", this.Query);
          this.StkQuery();
        }
      },
      immediate: true, // This ensures the watcher is triggered upon creation
    },

    timerEnabled3(value) {
      if (value) {
        setTimeout(() => {
          this.timerCount3--;
        }, 1000);
      }
    },
    timerCount3: {
      handler(value) {
        if (value > 0 && this.timerEnabled3) {
          setTimeout(() => {
            this.timerCount3--;
          }, 1000);
        } else if (value == 0) {
          this.FetchUserPin2();
        }
      },
      immediate: true, // This ensures the watcher is triggered upon creation
    },

    timerEnabled2(value) {
      if (value) {
        setTimeout(() => {
          this.timerCount2--;
        }, 1000);
      }
    },
    timerCount2: {
      handler(value) {
        if (value > 0 && this.timerEnabled2) {
          setTimeout(() => {
            this.timerCount2--;
          }, 1000);
        } else if (value == 0) {
          this.resetPin2();
        }
      },
      immediate: true, // This ensures the watcher is triggered upon creation
    },
  },
};
</script>

<style>
#all_items2 {
  --scrollbarBG: #000;
  --thumbBG: #2f2c2ce0;
  scrollbar-width: thin;
  scrollbar-color: var(--thumbBG) var(--scrollbarBG);
  overflow-y: scroll;
  width: 100%;
  align-items: start;
  bottom: 0;
  padding: 3px;
  height: 700px;
  justify-content: start;
}

#all_items2::-webkit-scrollbar {
  width: 8px;
}

#all_items2::-webkit-scrollbar-track {
  background: var(--scrollbarBG);
}

#all_items2::-webkit-scrollbar-thumb {
  background-color: var(--thumbBG);
  border-radius: 8px;
  border: 3px solid var(--scrollbarBG);
}

.blur {
  filter: blur(9px);
  /* Adjust the value (5px) to increase or decrease the blur intensity */
}

#charter {
  transition: 0.5s ease;
  background-image: url("@/assets/bg.png");
  background-attachment: fixed;
  background-position: center;
  background-size: contain;
  background-color: black;
  width: 100%;
  height: 260px;
}
</style>
