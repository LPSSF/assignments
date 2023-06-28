# Salesforce Utility Bar - Verify Contact  

For the voice inbound call, service agent always need to verify customer.

"From the utility Bar, service agents can quickly start the verification process. Please use flow to finish this function, and as LWC can provider more flexible and dynamic interaction. 
Some details:
1. the verification process will mark the case a field isCustomerVerified
2. in console, case page, agent can easily see the verification status 
3. verify code send to email or mobile, text code generation button's not need to call 3rd part API firstly "

Please design and imply the verification function with Flow and LWC.

The job can begin in a demo org to short the development time. 

1. LWC Components
1.1 VerifiContact.js
   ```
import { LightningElement, api } from 'lwc';
import verifyContact from '@salesforce/apex/VerifiContactUtil.verifyContact';
import verifyCode from '@salesforce/apex/VerifiContactUtil.verifyCode';
import sendCode from '@salesforce/apex/VerifiContactUtil.sendCode';


export default class VerifiContact extends LightningElement {
    @api contactId = '';
    @api caseId = '';
    
    @api contactFirstName = '';  
    @api contactLastName = '';  
    @api verifyResult = false;  // Output to flow

    verifyValue = '';

    get verifyOptions() {
        return [
            { label: 'Mobile', value: 'mobile' },
            { label: 'Email', value: 'email' },
        ];
    }

    error;
    // step 1, with given name and contact approach, verify the contact
    handleVerifyContact() { 
    }
    // step 2, send the verification code to the contact
    handleSendCode() { 
    }
    // step 3, verify the code
    handleVerifyCode() {
    }
}
   
   ```
