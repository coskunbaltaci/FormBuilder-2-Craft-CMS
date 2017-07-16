![Mou icon](http://vadim-dev.s3.amazonaws.com/formbuilder2/formbuilder2_logo.png)

***

FormBuilder 2 is a Craft CMS plugin that lets you create & manage forms for your front-end. Entries get stored to database so you can easily view your submission or export them.

***

# Front-End Template Usage

Take a look at `sampleForm.twig` for required code. You will need to copy everything in that file and paste it to where you want to display your form.

***

# Spam Protection

There are 2 options for spam protection, Time Submissions and Honeypot method.

#### 1. Time Submission

Time submission will prevent spam bots for submitting the form too quickly. You need to enter a time (seconds) it should take a real person to submit a form. Typically 3 seconds is good enough.

##### 2. Honeypot Method

With honeypot there is a hidden field that should be left blank. When spam bots run through the form they tend to fill out all the fields. If the honeypot field will get field the submission will fail. Real people will not be able to fill out this field so the form will submit successfully.


***

# File Uploads

Create an Asset field and add it to your form. You need to select **Has File Uploads** box when creating your form to make sure files get submitted. If you have Email Notifications turned on, file uploads will be attached to the email.

***

# Email Notifications

Templates have been added. Now you can visually set up email notification templates for admins or submitter. Currently with limited customization options but with feedback I'm sure we will add more. Check out this [video](https://www.youtube.com/watch?v=sRFgX_gQL5Y) for quick overlook. 

***



# Creating Forms

Hit `Create New Form` to create a new form.

* **Form Settings**
  * `Form Name` - Give your form a name
  * `Handle` - Will be autogenerated, you can change to a custom one
  * `Use Custom Redirect` - Select this option if you want a custom redirect page (thank-you page). Redirect URL is required and is relative to your domain.
  * `Has File Uploads` - Select this if your form has file uploads
  * `Use AJAX` - Select this if you want your form to submit using javascript without page refresh. `Note:` You can't use Has Form Uploads and Ajax together yet (working on it).
* **Spam Protection**
  * `Timed Submissions` - Select this and set time in seconds. Timed submissions will prevent quick spam robots from submitting too quickly. 
  * `Honeypot` - Select this option to try and catch spam robots using a honeypot method. Hidden field in the form needs to be left empty, if filled the form will fail (most robots will fill this field).
* **Messages**
  * `Success Message` - Enter form success message. If you form has custom redirect this is useless.
  * `Error Message` - Enter generic error message if submission has errors this will be displayed along with the field specific error message (`Ex: Your Name cannot be empty`).
* **Email Notifications**
  * `Notify Admin of Submission` - Select this if you want to send email notification of submission.
  * `Notification Email` - Enter email where the notification should be sent to. you can enter multiple emails separated by a comma `,`
  * `Email Subject` - Enter subject line for email notification
  * `Include Submission Data` - You can tick this option if you like the email to contain the submission content.
  * `Email Template` - You can pick if you want email to be Text Only or HTML Template.
  * `Text Only` - With this option you can enter optional `Body Copy` and `Footer Copy`.
  * `HTML Template` - With this option you can upload a `custom logo`, give email template a `custom background color`, give a `container width` as well as optional `Body Copy` and `Footer Copy`.
* **Fields**
  * `Add Fieldset Tab` - Click this to add a fieldset. Then drag and drop fields into those fieldsets. You can also drag predefined fieldsets with fields form the `Unused Fields` section. If you don't see any fields you will need to head over to `admin/settings/fields` to create some fields.
 
***

# Supported Fields

Here's a list of currently supported fields. Unlike FormBuilder there are no more custom fieldtypes, you only need to use Craft's predefined fields.

* Plain Text
* Checkboxes
* Radio Buttons
* Dropdowns (Select Options)
* Multi Select
* RichText (WYSIWYG)
* Light Switch (Toggle)
* Color (Color Picker)
* Date
* Number
* Assets (Only local file uploads supported for now, no s3, no rackspace)

***

# Custom Fields HTML Markup

If you want to have custom markup for your rendered fields follow these steps to achieve it.

* While editing your form go to the `Fields` tab. 
* Click the settings icon next your field and hit `Custom Template` option.
* Enter your template path located in `craft/templates`.
* Example: enter `forms/text`, place `text.html` or `text.twig` into `craft/templates/forms/`


***

# Custom Redirects

You can pass submission data to a custom redirect page, here is a snipped code for getting started.

```

{% set submissionId = craft.request.getCookie('formBuilder2SubmissionId') %}
{% set submission = null %}
{% if submissionId %}
  {% set submission = craft.formBuilder2.getFormById(submissionId.value) %}
{% endif %}

{% if submission %}
  {{ submission.form |inspect }}
  {{ submission.data |inspect }}
{% endif %}

```
* First we are getting a submission ID by checking cookies for it.
* If we get an ID we call a function to get the submission
* When and if you get a submission back you can use `submission.form` to get form information and `submission.data` to get submission information
  * `submission.form.id` - Form ID
  * `submission.form.title` - Form Name
  * `submission.data` - Holds submission data...so if your form had a field with handle name `yourEmail` you can call `submission.data.yourEmail` to get your string.

***

# Ajax Submission Event

You can use a custom `formbuilder2:submit` event to check for ajax submissions:

```
document.addEventListener("formbuilder:submit", formBuilderSubmission, false);
function formBuilderSubmission(e) {
	var details = e.details;
	var response = e.detail.response;
   	var success = e.detail.response.success;
}
```

For example, if you want to track successful submissions you can:

```
document.addEventListener("formbuilder:submit", successfulSubmission, false);
function successfulSubmission(e) {
   	var success = e.detail.response.success;
   	if (success) {
   		// add your tracking code here
   	}
}
```

***


## Todo

* Exporting entries
* Update documentations
* Visual data reporting

***


## Changelog

Refer to [releases.json](https://github.com/roundhouse/FormBuilder-2-Craft-CMS/blob/master/releases.json) for updates.




## Issue Tracking and Bug Reporting

If you have found a bug or would like to request a feature please use [Github's Issues](https://github.com/roundhouse/FormBuilder-2-Craft-CMS/issues) to report and track issues.




## License

[The MIT License (MIT)](http://opensource.org/licenses/MIT)

Copyright (c) 2014 Roundhouse Agency
Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:
The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.