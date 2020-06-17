# PayNet GoLang Rest API

PayNet rest API for GoLang is a package which implements the public API for create paypage of PayNet.
The package covers the following functions:
  - [authenticate_key] Authenticate key API
  - [create_pay_page] - Create Payment Page API
  - [verify_payment] - Verify Payment API

[![Build Status](https://travis-ci.com/samymassoud/PayNet_golang_api.svg?branch=master)](https://travis-ci.com/samymassoud/PayNet_golang_api)

# Installation
To install this package please run
> `go get github.com/Carnelian-Payment-Networks/paynet_golang_api_example`

# Create Payment Page
The concept behind the PayNet rest API is to create invoice using this API and redirect your merchant to that invoice to collect the payment and here is how to do it.
```golang
import (
	"fmt"
	"github.com/Carnelian-Payment-Networks/paynet_golang_api_example"
)

var data = make(map[string]string)
	data["merchant_email"] = "<MERCHANT_EMAIL>"
	data["secret_key"] = "<MERCHANT_SECRET>"
	data["currency"] = "USD"                     //change this to the required currency
	data["amount"] = "10"                        //change this to the required amount
	data["site_url"] = "<MERCHANT SITE>"         //change this to reflect your site
	data["title"] = "Sell products"              //Change this to reflect your order title
	data["quantity"] = "1"                       //Quantity of the product
	data["unit_price"] = "10"                    //Quantity * price must be equal to amount
	data["products_per_title"] = "Shoes | Jeans" //Change this to your products
	data["return_url"] = "<MERCHANT CALLBACK>"   //This should be your callback url
	data["cc_first_name"] = "Samy"               //Customer First Name
	data["cc_last_name"] = "Saad"                //Customer Last Name
	data["cc_phone_number"] = "00973"            //Country code
	data["phone_number"] = "12345678"            //Customer Phone
	data["billing_address"] = "Address"          //Billing Address
	data["city"] = "Manama"                      //Billing City
	data["state"] = "Manama"                     //Billing State
	data["postal_code"] = "1234"                 //Postal Code
	data["country"] = "BHR"                      //Iso 3 country code
	data["email"] = "<EMAIL>"                    //Customer Email
	data["ip_customer"] = "<IP>"                 //Pass customer IP here
	data["ip_merchant"] = "<IP>"                 //Change this to your server IP
	data["address_shipping"] = "Shipping"        //Shipping Address
	data["city_shipping"] = "Shipping"           //Shipping City
	data["state_shipping"] = "Shipping"          //Shipping State
	data["postal_code_shipping"] = "973"
	data["country_shipping"] = "BHR"
	data["other_charges"] = "0"                  //Other chargs can be here
	data["reference_no"] = "1234"               //Pass the order id on your system for your reference
	data["msg_lang"] = "en"                     //The language for the response
	data["cms_with_version"] = "Golang Lib v1"  //Feel free to change this

	resp, err := PayNet.CretaePayPage(data)

	if err != nil {
		fmt.Println(err)
	}

	if resp.ResponseCode == "4012" {
        //Paypage created, you can redirect the customer to this link for the payment
		fmt.Println(resp.PaymentURL)
	} else {
        //Paypage creation failed, you can check the reason here
		fmt.Println(resp.Result)
	}
```

# Verify Payment
After the payment is done you need to verify if it was successfull or not, so in the call back url PayNet will reply with Payment Reference and status, using this reference you can run the following code to make sure it's Paid.
``` golang
import (
	"fmt"
	"github.com/Carnelian-Payment-Networks/paynet_golang_api_example"
)

verifyMap := make(map[string]string)
verifyMap["merchant_email"] = "<MERCHANT_EMAIL>"
verifyMap["secret_key"] = "<MERCHANT_SECRET>"
verifyMap["payment_reference"] = "<ORDER_ID>"
result, err := PayNet.VerifyPayment(verifyMap)
if err != nil {
	println(err)
}

if result.ResponseCode == "100" {
	//Completed (Amount will be available,...)
	fmt.Println(result.Result)
} else {
	//Invalid
	fmt.Println(result.Result)
}
```
# Validate Secret Key
In some cases you will need to validate your secret key, for example if you are allowing to change the secret key infromation through your website control panel, so it's better to validate it before saving. This can be achived using this API
``` golang
import (
	"fmt"
	"github.com/Carnelian-Payment-Networks/paynet_golang_api_example"
)
validateMap := make(map[string]string)
validateMap["merchant_email"] = "<MERCHANT_EMAIL>"
validateMap["secret_key"] = "<MERCHANT_SECRET>"
result, err := PayNet.ValidateSecretKey(validateMap)
if err != nil {
	println(err)
}

if result.ResponseCode == "4000" {
	//Valid Secret Key
	println(result.Result)
} else {
	//Invalid
	println(result.Result)
}

```

# Note
You have to handle the call back URL through your application, in which PayNet will reply to your site with the payment result and then you can call verify payment api.

That's it.
Please use the package github page to report any issue or suggestion.