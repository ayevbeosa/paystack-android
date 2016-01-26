## paystack-android

This is a library for easy integration of Paystack with your Android application

## Installation

### Android Studio (using Gradle)
You do not need to clone this repository or download the files. Just add the following lines to your app's `build.gradle`:

    repositories {
      maven {
          url 'https://dl.bintray.com/paystack/maven/'
      }
    }
    dependencies {
      compile 'co.paystack.android:paystack:1.0.0'
    }

### Eclipse
To use this library with Eclipse, you need to:

1. Clone the repository.
2. Import the **Paystack** folder into your [Eclipse](http://help.eclipse.org/juno/topic/org.eclipse.platform.doc.user/tasks/tasks-importproject.htm) project
3. In your project settings, add the **Paystack** project under the Libraries section of the Android category.

## Usage

### 0. Prepare for use

To prepare for use, you must ensure to add the internet permissions to the AndroidManifest.xml

### 1. initializeSdk

To use the Paysack android sdk, you need to first initialize the sdk using the PaystackSdk class.

    PaystackSdk.initialize(getApplicationContext());

Make sure to call this method in the onCreate method of your Fragment or Activity.

### 2. setPublishableKey

Before you can create a token with the PaystackSdk, you need to set your publishable key. The library provides two approaches,

#### a. Add the following lines to the `<application></application>` tag of your AndroidManifest.xml
    <meta-data
        android:name="co.paystack.android.PublishableKey"
        android:value="your publishable key"/>

#### b. Set the publishable key by code
    PaystackSdk.setPublishableKey(publishableKey);


### 3. createToken
Creating a single-use token with the PaystackSdk is quite straightforward.

    //build a card
    Card card = new Card.Builder(cardNumber, expiryMonth, expiryYear, cvc).build();

    //create token
    PaystackSdk.createToken(card, new Paystack.TokenCallback() {
        @Override
        public void onCreate(Token token) {
            //retrieve the token, and send to your server for charging.
        }

        @Override
        public void onError(Exception error) {
            //handle error here
        }
    });

The first argument to the PaystackSdk.createToken is the card object. A basic Card object contains:

+ cardNumber: the card number as a String without any seperator e.g 5555555555554444
+ expiryMonth: the expiry month as an integer ranging from 1-12 e.g 10 (October)
+ expiryYear: the expiry year as an integer e.g 2015
+ cvc: the card security code as a String e.g 123

### 4. Charging the tokens. 
Send the token to your server and create a charge by calling our REST API. An authorization_code will be returned once the single-use token has been charged successfully. You can learn more about our API [here](https://developers.paystack.co/docs/getting-started).
 
 **Endpoint:** https://api.paystack.co/transaction/charge_token

 **Parameters:**
 

 - email  - customer's email address (required)
 - reference - unique reference  (required)
 - amount - Amount in Kobo (required) 

**Example**

    curl https://api.paystack.co/transaction/charge_token \
    -H "Authorization: Bearer SECRET_KEY" \
    -H "Content-Type: application/json" \
    -d '{"token": "PSTK_r4ec2m75mrgsd8n9", "email": "customer@email.com", "amount": 10000, "reference": "amutaJHSYGWakinlade256"}' \
    -X POST




**Result**

    {  "status": true,
       "message": "Charge successful",
       "data": {
                "amount": 10000,
                "transaction_date": "2016-01-26T15:34:02.000Z",
    "status": "success",
    "reference": "amutaJHSYGWakinlade256",
    "domain": "test",
    "authorization": {
      "authorization_code": "AUTH_d47nbp3x",
      "card_type": "visa",
      "last4": "1111",
      "bank": null
    },
    "customer": {
      "first_name": "John",
      "last_name": "Doe",
      "email": "customer@email.com"
    },
    "plan": 0}}




### 5. Charging Returning Customers
See details for charging returning customers [here](https://developers.paystack.co/docs/charging-returning-customers). 

 

### 6. Library aided card validation & utility methods
The library provides validation methods to validate the fields of the card.

#### card.validNumber
This method helps to perform a check if the card number is valid.

#### card.validCVC
Method that checks if the card security code is valid

#### card.validExpiryDate
Method checks if the expiry date (combination of year and month) is valid.

#### card.isValid
Method to check if the card is valid. Always do this check, before creating the token.

#### card.getType
This method returns an estimate of the string representation of the card type.

## Building the example project

1. Clone the repository.
2. Import the project either using Android Studio or Eclipse
3. Add your publishable key to your AndroidManifest.xml file
4. Build and run the project on your device or emulator

## Contact

For more enquiries and technical questions regarding the Android PaystackSdk, please send an email to androidsupport@paystack.co
