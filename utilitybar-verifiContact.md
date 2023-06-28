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

   1.2 VerifiContact.html

   ```
      <template>
          <lightning-record-edit-form object-api-name="Contact" record-id={recordId}>
          <lightning-messages></lightning-messages>
          <div class="slds-grid">
              <div class="slds-col slds-size_1-of-2">
                  <lightning-input-field field-name="Name"></lightning-input-field>
                  <lightning-input-field field-name="Title"></lightning-input-field>
              </div>
              <template>
                  <lightning-radio-group name="radioGroup" required label="Select the approach to send code" 
                                         options={verifyOptions} value={verifyValue} type="button">
                  </lightning-radio-group>
              </template>
              <div>
                  <input type="text" placeholder="Enter Mobile or Email" value={value}  />
                  <input type="button" label="Send" value={value} onchange={handleChange} />
              </div>
          </div>
          <div class="slds-col slds-size_1-of-2">
              <lightning-input-field field-name="Verify Code"></lightning-input-field>
          </div>
          <div class="slds-m-top_medium">
              <lightning-button  variant="brand" label="Verify Contact"></lightning-button>
              <lightning-button  variant="brand-outline" label="Cancel"></lightning-button>
          </div>
      </template>
   
   ```

   1.3 VerifiContact.js-meta.xml
   ```
      <?xml version="1.0" encoding="UTF-8"?>
      <LightningComponentBundle xmlns="http://soap.sforce.com/2006/04/metadata">
          <apiVersion>57.0</apiVersion>
          <description>Verify Contact</description>
          <isExposed>true</isExposed>
          <masterLabel>Verify Contact</masterLabel>
          <targets>
              <target>lightning__Tab</target>
              <target>lightning__FlowScreen</target>
          </targets>
          <targetConfigs>
              <targetConfig targets="lightning__FlowScreen">
                  <property name="caseId" label="Case ID" type="String" role="inputOnly"/>
                  <property name="caseContactVerified" label="Contact Verified for this Case" type="boolean"/>
              </targetConfig>
          </targetConfigs>
      </LightningComponentBundle>
   ```

   2. Apex VerifiContactUtil for support the frontend
  
   ```
         public with sharing class VerifiContactUtil {
             
             @AuraEnabled
             global static boolean verifyContact(String caseId, String firstName, String lastName, String email, String phone, String mobileOrEmail) {
                 if (mobileOrEmail != null && firstName != null && lastName != null ) {
                     List<Contact> contacts = [SELECT Id, FirstName, LastName, Email, Phone, MobilePhone FROM Contact WHERE Phone = :phone OR MobilePhone = :phone];
                     if (contacts.size() > 0) {
                         return true;
                     } else {
                         return false;
                     }
                 } else {
                     return false;
                 }
             }
         
             @AuraEnabled
             global static String sendCode(String mobileOrEmail, String email, String phone) {
                 // send code to mobile or email
                 // dummy code 
                 return '123456'
             }
         
             @AuraEnabled
             global static boolean verifyCode(String code) {
                 // verify code
                 // dummy code
                 if (code == '123456') {
                     return true;
                 } else {
                     return false;
                 }
             }
         }

   
   ```

### References:
1. How  LWC be a screen of FLOW
Configure a Component for Flow Screens https://developer.salesforce.com/docs/component-library/documentation/en/lwc/lwc.use_config_for_flow_screens
2. Lightning Design System
https://www.lightningdesignsystem.com/

