### Using Webhooks for Google Forms to Send Notifications to Discord

#### Setting up a Webhook in Discord

1. **Create a Discord Webhook**:
   - Go to your Discord server or channel where you want to receive notifications.
   - Right-click on the channel and select "Edit Channel".
   - Go to the "Webhooks" tab and click on "Create Webhook".
   - Customize the name, avatar, and channel for your webhook.
   - Copy the webhook URL provided.

#### Integrating Google Forms with Discord Webhook

1. **Open Google Form**:
   - Go to [Google Forms](https://forms.google.com/) and open the form you want to set up notifications for.

2. **Open Google Apps Script**:
   - Click on the "More options" (three vertical dots) in the top-right corner of your Google Form.
   - Select "Script editor". This will open Google Apps Script associated with your form.

3. **Write Apps Script Code**:
   ```javascript
   function onSubmit(e) {
     var formData = e.namedValues;
     var webhookUrl = "YOUR_DISCORD_WEBHOOK_URL";
     
     var message = "New form submission:\n";
     for (var field in formData) {
       message += field + ": " + formData[field] + "\n";
     }
     
     var payload = {
       content: message
     };
     
     var options = {
       method: "post",
       contentType: "application/json",
       payload: JSON.stringify(payload)
     };
     
     UrlFetchApp.fetch(webhookUrl, options);
   }
   ```
   Replace `"YOUR_DISCORD_WEBHOOK_URL"` with the Discord webhook URL you obtained earlier.

4. **Save and Deploy Script**:
   - Save the script and give it a name.
   - Deploy the script as a web app:
     - Click on the Deploy icon (next to the floppy disk icon) and select "New deployment".
     - Choose "Web app" as the deployment type.
     - Set access to "Anyone, even anonymous".
     - Click "Deploy".
     - Authorize the script to run if prompted.

5. **Connect Google Form to Webhook**:
   - Go back to your Google Form.
   - Click on the "Responses" tab.
   - Select the "More options" (three vertical dots) menu.
   - Choose "Script editor".
   - In the Apps Script editor, select the function you created (`onSubmit`) and click "Save".

6. **Test**:
   - Test your integration by submitting a form entry.
   - Check if your Discord channel receives the notification with the form submission data.

With this setup, every time someone submits the Google Form, the data will be sent to the designated Discord channel via the webhook, providing real-time updates and notifications.

#### Information Extraction and Format from Google Apps Script

From the event object passed to the `onFormSubmit` trigger function in Google Apps Script, you can extract various information about the submitted form response. The event object contains information about the submitted form, such as the form response, the timestamp of the submission, and the form item titles and responses.

Here's an example of the structure of the event object in Google Apps Script:

```javascript
{
  authMode: AuthMode,
  values: string[],
  namedValues: object,
  range: Range,
  triggerUid: string,
  authMode: AuthMode
}
```

- `authMode`: Represents the authorization mode used to run the trigger function.
- `values`: An array of all the values in the submitted form response.
- `namedValues`: An object containing key-value pairs where the keys are the titles of form items and the values are the corresponding responses.
- `range`: The range in the spreadsheet that contains the submitted form response.
- `triggerUid`: A unique identifier for the trigger that triggered the function.

**Google Form Event Object Values Descriptions**

1. **Form Response Values** (`values`):
   - This is an array containing all the values in the submitted form response. Each element in the array corresponds to a single question or item in the form, in the order they appear in the form.
   - Example: `["John Doe", "25", "Male", "Los Angeles", "Yes"]`

2. **Named Values** (`namedValues`):
   - This is an object containing key-value pairs where the keys are the titles of form items (questions) and the values are the corresponding responses.
   - Example:
     ```javascript
     {
       "Full Name": "John Doe",
       "Age": "25",
       "Gender": "Male",
       "Location": "Los Angeles",
       "Subscribe to Newsletter": "Yes"
     }
     ```

3. **Range** (`range`):
   - This represents the range in the spreadsheet that contains the submitted form response. You can use this to further manipulate the data in the spreadsheet if needed.
   - Example: `Sheet1!A2:E2`

4. **AuthMode** (`authMode`):
   - This represents the authorization mode used to run the trigger function. It indicates whether the function was run with the authorization of the active user or the triggering user.
   - Possible values: `NONE`, `FULL`, `LIMITED`, `CUSTOM_FUNCTION`

5. **Trigger UID** (`triggerUid`):
   - This is a unique identifier for the trigger that triggered the function. It can be useful for debugging purposes or tracking the trigger event.

With this information, you can perform various actions based on the submitted form response, such as sending emails, updating spreadsheets, generating reports, or making HTTP requests to external services. By accessing the form response data in Google Apps Script, you can automate workflows and integrate Google Forms with other systems or services effectively.

#### Discord Format
When integrating a Google Form with Discord using webhooks, the data sent to the Discord channel would typically be a formatted message containing the information submitted through the form. The exact format of the message depends on how you structure the content in your Google Form and how you handle it in your webhook script.

Here's an example of what the data in the Discord channel might look like based on the information submitted through a Google Form:

```
New Form Submission:
- Full Name: John Doe
- Age: 25
- Gender: Male
- Location: Los Angeles
- Subscribe to Newsletter: Yes
```

In this example, each form field and its corresponding response are listed in a structured format, making it easy to read and understand the submitted information. You can customize the message format according to your preferences and requirements.

Additionally, you may include additional formatting, such as bold text or inline code blocks, to enhance the readability and presentation of the data in the Discord channel. The specific formatting options available depend on the capabilities of Discord's webhook feature and the Markdown syntax supported in Discord messages.
