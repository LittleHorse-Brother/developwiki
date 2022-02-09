# Visitor side Javascript API

### Get Started

#### Where to define code

To use Comm100 Live Chat API, you can either write the code on your own page, or define the code in your Control Panel. If you define the code on your own page, the code will be available for chat button, invitation and embedded chat window. However, pop up chat window is not included. If you would like to use API for pop up chat window as well, you need to define the code in your Control Panel.

To define code in your Control Panel, please first login [here](https://www.comm100.com/secure/login.aspx), then goes to "LiveChat" >> "Campaign" >> "Chat Window" >> "Advanced" >> "Add custom JavaScript to my chat window".

#### How to call API

Comm100 Live Chat API needs time to load. You can only use APIs when it's fully ready. Thus, it's recommended for you to wrap your call with a callback function that is assigned to `Comm100API.onReady`.

Comm100 Live Chat will trigger this callback when everything is ready.

Example as shown below:

```javascript
Comm100API.onReady = function () {
  // use API here safely
};
```

#### What APIs provided

Comm100 Live Chat provides 5 fundamental APIs to help you gain control of your chat on site.

**Get**

To get necessary information, you could use `Comm100API.get`:

```javascript
/**
 * @param {string} type - Type of data to get, you can find all types in doc below.
 * @param {...any} args - Any specific argument required.
 * @return {any} result.
 */
 Comm100API.get('type', ...args);
```

For example,

```javascript
Comm100API.get('livechat.button.isVisible', campaignId);
```

will tells you whether button with campaign ID equals to `campaignId` is visible.

**Set**

To set info into chat system, you could use `Comm100API.set`:

```javascript
/**
 * @param {string} type - Type of data to set, you can find all types in doc below.
 * @param {any} value - Value that will be set.
 * @param {...any} args - Any additional arguments required by API.
 */
Comm100API.set('type', value, ...args);
```

For example,

```javascript
Comm100API.set('livechat.button.isVisible', campaignId, true);
```

will set button with campaign ID equals to `campaignId` as visible.

**Event**

If you would like to listen to specific event, you could use `Comm100API.on`:

```javascript
/**
 * @param {string} type - Type of event you would like to listen to, you can
 * find all types in doc below.
 * @param {function} callback - Subscription function, which will be triggered when
 * event fires.
 * @return {function} unregistration - Unsubscribe callback, stops listening to
 * this event.
 */
Comm100API.on('type', callback);
```

For example,

```javascript
Comm100API.on('livechat.dynamicCampaign.change', function (currentCampaignId) { });
```

will register subscription function which listens to dynamic campaign changes. It will be triggered whenever campaign ID changes, and current campaign ID will be provided as parameter.

**Replace Element**

Comm100 Live Chat provides custom CSS feature to allow modification of live chat components. If you would like to use your own component instead of modifying the style of existing one, you could use `Comm100API.render`:

```javascript
/**
 * @param {string} type - Type of component you would like to replace, you can find
 * all types in doc below.
 * @param {function} callback - Callback function which will be used to generate
 * custom component for replacement. If HTMLElement is returned in this callback,
 * it will be used to replace default component; If falsy value is returned, default
 * component will be used instead.
 */
Comm100API.render('type', callback);
```

For example,

```javascript
Comm100API.render('livechat.invitation.popupImage', function () {
  const customComponent = document.createElement('div');
  return customComponent;
});
```

will replace image invitation with custom component returned by this callback function.

**Act**

You might want to manually trigger some actions, instead of waiting for visitor to do it. To do this, you could use `Comm100API.do`:

```javascript
/**
 * @param {string} type - Type of action to do, you can find all types in doc below.
 * @param {...any} args - Any argument required.
 * @return {Promise<void> | void} Promise which indicates whether async action is
 * finished successfully or not. For sync actions, nothing will be returned.
 */
Comm100API.do('type', ...args);
```

For example,

```javascript
Comm100API.do('livechat.button.click', campaignId);
```

will trigger action, the same as visitor click on button with campaign ID `campaignId`.

#### Examples

Below are two typical examples of how to use custom JavaScript.

**Dynamically Change Options based on User Input**

One powerful feature of JavaScript is that it allows you to implement inter-dependent select menus, where the select of one menu changes the contents of another.

For example, you have placed a Product Service field (dropdown list) on your pre-chat form. When visitors select Live Chat, the 4th Field will show; when visitors select other options in the Product Service field, the 5th Field will show.

```javascript
/**
 * Display either 4th Field or 5th Field based on the select result in Product Service field.
 */
Comm100API.onReady = function () {
  function fieldDisplay(field) {
    // check if current field is Product Service
    if (field.label !== 'Product Service') return;
    var fields = Comm100API.get('livechat.prechat.fields');
    // check if current selected value is 'livechat'
    if (field.value === 'livechat') {
      // display/hide fields accordingly
      fields[3].isHidden = false; // 4th field
      fields[4].isHidden = true;  // 5th field
    } else {
      fields[3].isHidden = true;
      fields[4].isHidden = false;
    }
    Comm100API.set('livechat.prechat.fields', fields);
  }
  Comm100API.on('livechat.prechat.field.change', fieldDisplay);
  Comm100API.on('livechat.prechat.display', function () {
    var fields = Comm100API.get('livechat.prechat.fields');
    fields.forEach(fieldDisplay);
  });
};
```

**Dynamically Insert Custom Fields and Submit**

Imagine that you could insert custom fields in your post-chat survey, and then the collected visitors' feedback would also be available in your Comm100 account.

Here is how custom JavaScript can help you:

Note: To make sure that Comm100 server can store the value of your custom fields correctly, the value your visitors input in custom fields needs to be insert into Comm100 Post Chat fields before submit. This means that you need to add the corresponding fields in Comm100 Post Chat Window, mapping with your own custom fields. Click [here](https://www.comm100.com/livechat/knowledgebase/how-to-customize-the-post-chat-window.html) to learn how to add fields to your post-chat window.

```javascript
Comm100API.onReady = function () {
  /**
   * Set specific value in field named text
   * @param {string} val - Value that will be set into field
   */
  function setValue(value) {
    var fields = Comm100API.get('livechat.postChat.fields');
    Comm100API.set('livechat.postChat.fields', fields.map(function (field) {
      if (field.label !== 'text') return field;
      field.value = value;
      return field;
    }));
  }
  /** Use custom fields in post chat intead of default one */
  Comm100API.render('livechat.postChat.form', function () {
    /** Create custom fields */
    var container = document.createElement('div');
    container.id = 'customPostChatContainer';
    container.innerHTML =
      '<table><tr><td>Post Example:</td><td><input id="customerName"/></td></tr></table>';
    /** Register event callbacks */
    container.getElementsByTagName('input')[0].addEventListener('change', function () {
      /** Save visitors' input back to Comm100 post chat fields */
      setValue(this.value);
    }, true);
    /** Render custom fields */
    return container;
  });
};
```

### General

#### Site ID

You can use API to get ID of your site, example is shown below:

```javascript
/** @type {number} */
const siteId = Comm100API.get('livechat.siteId');
```

#### All Campaign ID

You can use API to get all IDs of campaign used on current page. Notice that the API returns an array of IDs as you might have multiple campaigns installed on current page.

```javascript
/** @type {Array<string>} */
const campaignIds = Comm100API.get('livechat.campaignIds');
```

#### Dynamic Campaign

**Get**

You can use API to determine whether dynamic campaign feature is enabled on current page.

```javascript
/** @type {boolean} */
const isDynamicCampaign = Comm100API.get('livechat.isDynamicCampaign');
```

**Event**

When dynamic campaign is enabled, the campaign used on your page might vary due to condition changes. You could register a callback to listen to such changes, as shown below:

```javascript
/**
 * @param {string} currentCampaignId - Current Campaign ID
 */
function subscribe(currentCampaignId) {
  // do stuff here
}
const unsubscribe = Comm100API.on('livechat.dynamicCampaign.change', subscribe);
```

Please notice that this API will return an unregistration function, which can be used to stop listening to this event.

#### Mobile

You can use API to determine if current visitor is using Mobile device to browse your website. Notice that Comm100 Live Chat will have different behavior for mobile devices. This API will tell you whether these behavior for mobile has been enabled or not.

```javascript
/** @type {boolean} */
const isMobile = Comm100API.get('livechat.isMobile');
```

#### Custom Variable

You can use API to manually set custom variables. To do this, you need to provide an array of custom variables that you wish to set. Each element inside array should be an object containing `name` and `value` attribute, where `name` should be the name of custom variable you defined in Control Panel, and `value` is it's current value.

Please notice that `value` should be a string, otherwise to will be converted to string using built-in `toString()`. Also, if `undefined` is assigned to `value`, it will be ignored and will not update this custom variable.

Example is shown below:

```javascript
const values = [
  {
    name: 'custom variable 1',
    value: 'current value',
  },
  {
    name: 'non-existant custom variable',
    value: 'current value',
  },
  {
    name: 'ignored custom variable',
  },
];
Comm100API.set('livechat.customVariables', values);
```

In example above, custom variable `custom variable 1` will be updated using value `current value`, while `non-existant custom variable` will not be updated if it's not defined in Control Panel. Also, custom variable `ignored custom variable` will not be updated as it's value is `undefined`.

#### Visitor

You can use API to get the GUID (Globally Unique Identifier) of current visitor. The GUID is ensured to be different amoung each visitor.

```javascript
/** @type {string} */
const guid = Comm100API.get('livechat.visitor.guid');
```

### Chat Button

#### Visibility

**Get**

To know whether a specific button is visible or not, you could use the following API:

```javascript
/** @type {string} */
const campaignId = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx';
/** @type {boolean} */
const isVisible = Comm100API.get('livechat.button.isVisible', campaignId);
```

You need to provide campaign ID of button you are interested in. If no campaign id is provided, the first campaign ID returned by `Comm100API.get('livechat.campaignIds')` will be used as default value.

**Set**

Meanwhile, you could update visibility of button as well:

```javascript
/** @type {boolean} */
const isVisible = true;
/** @type {string} */
const campaignId = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx';
Comm100API.set('livechat.button.isVisible', isVisible, campaignId);
```

Similarly, if campaign ID is not provided, the first campaign ID returned by `Comm100API.get('livechat.campaignIds')` will be used as default value.

**Event**

You could also listen to changes of visibility:

```javascript
Comm100API.on('livechat.button.show', function (button) { });
Comm100API.on('livechat.button.hide', function (button) { });
```

The first callback will be triggered when one button is shown; The second callback will be triggered when one button is hidden. When callback is triggered, info of chat button will be provided as an object, which looks like:

```javascript
const button = {
  id: 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', // string, campaign id of button
  type: 'adaptive', // string, type of button
};
```

Possible types of chat buttons are: `"adaptive"`, `"float"`, `"image"`, `"text"`:

* `"adaptive"`: Adaptive button
* `"float"`: Float image button
* `"image"`: Static image button
* `"text"`: Text button

#### Online Status

**Get**

To know whether specific button is online or offline, you could use:

```javascript
/** @type {string} */
const campaignId = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx';
/** @type {"online" | "offline"} */
const status = Comm100API.get('livechat.button.status', campaignId);
```

If campaign ID is not provided, the first campaign ID returned by `Comm100API.get('livechat.campaignIds')` will be used as default value. The return value of this API is string, either `"online"` or `"offline"`.

**Event**

You could use the following APIs to listen to changes of button online status:

```javascript
Comm100API.on('livechat.button.online', function (button) { });
Comm100API.on('livechat.button.offline', function (button) { });
```

Informaiton of button will be provided as an object. The structure of this object is the same as `"livechat.buttons.show"` event, which can be found [here](broken-reference)

#### Open Chat Window

You can only specific chat window by clicking on corresponding chat button, or use the following API:

```javascript
/** @type {string} */
const campaignId = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx';
Comm100API.do('livechat.button.click', campaignId);
```

If `campaignId` is not proivded, the first campaign ID returned by `Comm100API.get('livechat.campaignIds')` will be used as default value.

This is a sync action, thus no return value for this API.

Please also notice that if there is already an opened chat window, calling this API might trigger nothing.

### Invitation

#### Display

**Event**

Comm100 Live Chat API provides event when invitation shows:

```javascript
Comm100API.on('livechat.invitation.display', function (invitation) { });
```

Here, `invitation` is an object containing useful info about current invitation:

```javascript
const invitation = {
  id: 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx',          // string, invitation id
  campaignId: 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx',  // string, campaign id of this invitation
  message: '',    // string, invitation text
  isManual: true, // boolean, whether current invitation is manual invitation
  agent: {        // agent will be provided if it is manual invitation
    id: 0,        // number, agent id
    name: '',     // string, name of agent
    avatar: '',   // string, url of avatar image of this agent
    title: '',    // string, title of agent
    bio: '',      // string, bio of agent
  },
};
```

**Replace**

Instead of using default image invitation, you could also use API to replace it with your own version:

```javascript
Comm100API.render('livechat.invitation.popupImage', function (invitation) {
  const component = document.createElement('div');
  // do things here
  return component;
});
```

The callback will be triggered with `invitation` object provided, the same structure as shown [above](broken-reference). To replace it with your own version, you need to return an HTMLElement. If falsy value is returned, default invitation will be used and skip replacement process.

#### Accept Invitation

**Event**

To receive events when visitor accept invitation, use:

```javascript
Comm100API.on('livechat.invitation.accept', function (invitationId) { });
```

`invitationId` as string will be provided as parameter, it is the id of current invitation.

**Action**

You could also simulate accept invitation action using the following API:

```javascript
Comm100API.do('livechat.invitation.accept', invitationId);
```

You need to provide invitation id string of available invitation when accepting invitaiton, otherwise API will ignore the call.

#### Refuse Invitation

**Event**

To receive events when visitor refuse invitation, use:

```javascript
Comm100API.on('livechat.invitation.refuse', function (invitationId) { });
```

`invitationId` as string will be provided as parameter, it is the id of current invitation.

**Action**

You could also simulate refuse invitation action using the following API:

```javascript
Comm100API.do('livechat.invitation.refuse', invitationId);
```

You need to provide invitation id string of available invitation when refusing invitaiton, otherwise API will ignore the call.

### Pre-Chat

#### Skip PreChat

To skip pre chat:

```javascript
/** @type {boolean} */
const isPrechatSkipped = true;
Comm100API.set('livechat.prechat.isSkip', isPrechatSkipped);
```

Please notice that prechat forms need to be filled with correct data to ensure that skipping prechat can work. That means, if there is any required field with empty value, or if there is invalid email address in email field, etc., skipping prechat will fail and prechat will still be shown.

To fill prechat form with data, please check the API [below](broken-reference).

To see if pre-chat has been set to skip:

```javascript
/** @type {boolean} */
const isPrechatSkipped = Comm100API.get('livechat.prechat.isSkip');
```

#### Fields

**Get All Fields**

You could get all fields in prechat using API:

```javascript
/** @type {string} */
const campaignId = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx';
/** @type {Array<Field>} */
const fields = Comm100API.get('livechat.prechat.fields', campaignId);
```

Campaign ID is optional parameter that can be used to determine which campaign you would like to get. If it is not provided, the first campaign ID returned by `Comm100API.get('livechat.campaignIds')` will be used as default value.

Here, the API will return a list of fields, each is an object representing the field in form, which has following structure:

```javascript
const field = {
  id: 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx',  // string, unique field id
  value: 'xxx',         // string/File, value of field. File for attachment, string for others.
  label: 'xxx',         // string, label of field
  type: 'xxx',          // string, type of field, all types are listed below
  error: 'xxx',         // string, error message
  isErrorShowed: false, // boolean, whether error is displayed
  isHidden: false,      // boolean, whether field is invisible
  isRequired: false,    // boolean, whether field is required
  isFocused: false,     // boolean, whether field is focused
  options: [            // array, required for fields such as checkbox, radio list, select
    {
      label: 'xxx',     // string, label name
      value: 'xxx',     // string, value
    },
  ],
}
```

Possible field types are:

* `"name"`: Name field (system field)
* `"email"`: Email field (system field)
* `"phone"`: Phone field (system field)
* `"company"`: Company field (system field)
* `"product"`: Product and Service field (system field)
* `"department"`: Department field (system field)
* `"ticket"`: Ticket field (system field)
* `"rating"`: Rating field (system field)
* `"comment"`: Comment field (system field)
* `"subject"`: Subject field (system field)
* `"content"`: Content field (system field)
* `"attachment"`: Attachment field (system field)
* `"text"`: Text field (custom field)
* `"textarea"`: Textarea field (custom field)
* `"radio"`: Radio Box field (custom field)
* `"checkbox"`: Check Box field (custom field)
* `"select"`: Drop Down List field (custom field)
* `"checkbox-list"`: Check Box List field (custom field)

**Set All PreChat Fields**

Similar as getting all fields for prechat, you could also use API to set all fields for prechat, including it's value, error messages, etc.:

```javascript
/** @type {string} */
const campaignId = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx';
/** @type {Array<Field>} */
const fields = [];
Comm100API.set('livechat.prechat.fields', fields, campaignId);
```

Campaign ID is optional parameter that can be used to determine which campaign you would like to get. If it is not provided, the first campaign ID returned by `Comm100API.get('livechat.campaignIds')` will be used as default value.

Besides all types of fields listed above, you could also set custom field if system provided types cannot fulfill all your requirement:

```javascript
const fields = [{
  id: 'custom',
  type: 'custom', // type of field
  element: function (update, error) {
    const component = document.createElement('input');
    component.onchange = function (event) {
      const value = event.target.value;
      if (value.trim() !== '') {
        error(null);
        update(value);
      } else {
        error('Custom Error Here');
      }
    };
    return component;
  },
}];
Comm100API.set('livechat.prechat.fields', fields);
```

In example above, a custom field is defined.

The attribute `element` provides a callback function which will be used to render element. It should return an HTMLElement which will be placed in form. When callback is triggered, `update` as a function will be provided. Custom component should use this `update` function to inform the system whenever the value of field has been changed: `update(value);`.

Also, it provides second param `error`. This is also a function, that should be used when you wish to update error of this custom field. Updating error in this custom field will not make it shown any difference, because yourself have all the control. However, inform the system that this field has error will prevent default submit action, so that you can ensure custom will not pass form when custom field is still invalid. Please be sure to remove error by calling `error(null)` when field is valid.

Also, please notice that it provides `field.id` as string in above example. If field id is not provided as string, this field will not be submitted to server. You can use this to add your own validation field. However, if you would like the data to be submitted, please add a field id which matches any existing field id customized in Control Panel, and it must be a string.

**Field Change Event**

To listen to change event of fields, use following API:

```javascript
Comm100API.on('livechat.prechat.field.change', function (field) { });
```

`field` will be provided when event fires, the structure of this object can be found [here](broken-reference).

**Blur and Focus**

To focus on specific field, use following API:

```javascript
/** @type {string} */
const fieldId = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx';
Comm100API.do('livechat.prechat.field.focus', fieldId);
```

To listen to focus/blur event, use one of the following APIs:

```javascript
Comm100API.on('livechat.prechat.field.focus', function (field) { });
Comm100API.on('livechat.prechat.field.blur', function (field) { });
```

`field` will be provided when event fires, the structure of this object can be found [here](broken-reference).

#### Submit Form

**Event**

When visitor submit prechat form, corresponding event will be triggered. Following API shows how to listen to this event:

```javascript
Comm100API.on('livechat.prechat.submit', function (formData) { });
```

Here, `formData` is an object containing all values of fields in prechat form. The structure is as follows:

```javascript
const formData = {
  name: '', // string, value of name field (i.e. field with type as "name")
  email: '', // string, value of email field (i.e. field with type as "email")
  phone: '', // string, value of phone field (i.e. field with type as "phone")
  company: '', // string, value of company field (i.e. field with type as "company")
  product: '', // string, value of product field (i.e. field with type as "product")
  department: '', // string, id of selected department, '' for not selected
  ticketId: '', // string, value of ticket field (i.e. field with type as "ticket")
  rating: '', // string, value of rating field (i.e. field with type as "rating")
  comment: '', // string, value of comment field (i.e. field with type as "comment")
  subject: '', // string, value of subject field (i.e. field with type as "subject")
  content: '', // string, value of content field (i.e. field with type as "content")
  attachment: File, // File, value of attachment field (i.e. field with type as "attachment")
  custom: { // custom fields, where key is id of the field, and value is field's value
  },
};
```

Info in `formData` are all optional and will only be provided if corresponding field is available in prechat form.

This event will be triggered when visitor submits the form.

When submitting is succeed, following event will be triggered:

```javascript
Comm100API.on('livechat.prechat.submit.success', function () { });
```

However, if server reject it, following event will be triggered:

```javascript
Comm100API.on('livechat.prechat.submit.fail', function (message) { });
```

where `message` is a string indicating the error message.

**Submit Action**

You could also use API to submit prechat form directly:

```javascript
/** @type {Promise<void>} */
const promise = Comm100API.do('livechat.prechat.submit');
```

The chat window requires to be opened when submitting the form. Calling this API is equivalent to clicking the submit button by user. That means, if any of the fields has issue, submit will fail. Hence, for this API, a promise will be returned, indicating whether submit success or not. If promise is rejected, a list of IDs will be provided as first parameter, indicating which fields are invalid due to incorrect values; if first parameter is `null`, you could check the second parameter for possible error message.

#### Display

You could listen to display event of prechat form:

```javascript
Comm100API.on('livechat.prechat.display', function () { });
```

#### Minimize

You could listen to minimize event of prechat form:

```javascript
Comm100API.on('livechat.prechat.minimize', function () { });
```

To minimize the form manually, use following API:

```javascript
Comm100API.do('livechat.prechat.minimize');
```

#### Restore

You could listen to restore event of prechat form:

```javascript
Comm100API.on('livechat.prechat.restore', function () { });
```

To restore the form manually, use following API:

```javascript
Comm100API.do('livechat.prechat.restore');
```

#### Close

You could listen to close event of prechat form:

```javascript
Comm100API.on('livechat.prechat.close', function () { });
```

To close the form manually, use following API:

```javascript
Comm100API.do('livechat.prechat.close');
```

#### Replacement

There are several components in prechat form which can be replaced by your own component, following are they and how you could replace:

**Header Icons**

```javascript
Comm100API.render('livechat.prechat.header.icons', function () {
  // return custom HTMLElement to replace default component
});
```

**Header**

```javascript
Comm100API.render('livechat.prechat.header', function () {
  // return custom HTMLElement to replace default component
});
```

**Greeting Message Area**

```javascript
Comm100API.render('livechat.prechat.greeting', function () {
  // return custom HTMLElement to replace default component
});
```

**Social Login Area**

```javascript
Comm100API.render('livechat.prechat.social', function () {
  // return custom HTMLElement to replace default component
});
```

**Prechat Form**

```javascript
Comm100API.render('livechat.prechat.form', function () {
  // return custom HTMLElement to replace default component
});
```

**Submit Button**

```javascript
Comm100API.render('livechat.prechat.submit', function () {
  // return custom HTMLElement to replace default component
});
```

**Footer**

```javascript
Comm100API.render('livechat.prechat.footer', function () {
  // return custom HTMLElement to replace default component
});
```

**Powered-by Area**

```javascript
Comm100API.render('livechat.prechat.footer.poweredby', function () {
  // return custom HTMLElement to replace default component
});
```

### Chat

#### Chat Status

You could use the following API to listen to chat status changes:

```javascript
Comm100API.on('livechat.chat.start', function (chatGuid) { });
Comm100API.on('livechat.chat.end', function () { });
```

Here, when chat starts, the callback will be triggered with chat ID provided (as string). Chat will be considered as started when agent accepts the requesting chat, either accepts manually or automatically.

When visitor is waiting, the chat is considered not started yet. You could use the following API to listen to the queue position change:

```javascript
Comm100API.on('livechat.chat.waitingQueue.update', function (queue) { });
```

Here, queue position data will be provided as an object when callback is triggered. The structure of this object is as follows:

```javascript
const queue = {
  position: 0, // number, position of queue the visitor is in
  waitTime: 0, // number, estimated wait time
};
```

#### Agent

Comm100 Live Chat API provides several events related to agents involved in chats, they are:

**Agent Join Chat**

```javascript
Comm100API.on('livechat.chat.agent.join', function (agent) { });
```

Here, agent info is provided as an object when callback is triggered. The structure of this object is as follows:

```javascript
const agent = {
  id: 0, // number, agent id
  name: '', // string, agent name
  title: '', // string, title of agent
  bio: '', // string, bio of agent
};
```

**Agent Leave Chat**

```javascript
Comm100API.on('livechat.chat.agent.leave', function (agent) { });
```

Here, agent info is provided as an object when callback is triggered. The structure of this object is the same as shown [above](broken-reference).

**Agent Transfer Chat**

```javascript
Comm100API.on('livechat.chat.agent.transfer', function (agent) { });
```

Here, agent info is provided as an object when callback is triggered. The structure of this object is the same as shown [above](broken-reference).

Please notice that this agent is the one who transfer the chat to someone else or to some other department. If you would like to know who accept the transfer, you should use [join chat](broken-reference) API instead.

**Agent Input**

```javascript
Comm100API.on('livechat.chat.agent.input', function (agent, content) { });
```

Here, agent info is provided as an object when callback is triggered. The structure of this object is the same as shown [above](broken-reference).

It also provides second parameter, which is the content of agent sent message. For text message, `content` will be string; for attachment or image, `content` will be an object with property `name` indicating the name of this attachment and `url` indicating the download link of this attachment.

#### Visitor

**Visitor Input**

```javascript
Comm100API.on('livechat.chat.visitor.input', function (content) { });
```

When visitor sends out text messages, this event will be triggered. When function is called, what visitor sends out will be provided as `content` parameter. Here, `content` is string.

Please notice that if you configured to mask credit card number, the sensitive data will be masked first before providing to you via API.

**Visitor Set Email**

```javascript
Comm100API.on('livechat.chat.visitor.setEmail', function (email) { });
```

This api will inform you when visitor set his/her email address during the chat. Email address will be provided as string.

**Rating**

```javascript
Comm100API.on('livechat.chat.visitor.rate', function (rating) { });
```

When visitor rate the agent during chat, callback will be triggered. The rating result will be provided as an object, which has the following structure:

```javascript
const rating = {
  score: 0, // number, rating score
  comment: '', // string, rating comment
}
```

Please notice that the attribute of strucutre above are optional, it depends on whether you have enabled them in Control Panel.

#### Display

You could listen to display event of chat panel:

```javascript
Comm100API.on('livechat.chat.display', function () { });
```

#### Minimize

You could listen to minimize event of chat panel:

```javascript
Comm100API.on('livechat.chat.minimize', function () { });
```

To minimize chat panel manually, use following API:

```javascript
Comm100API.do('livechat.chat.minimize');
```

#### Restore

You could listen to restore event of chat panel:

```javascript
Comm100API.on('livechat.chat.restore', function () { });
```

To restore the chat panel manually, use following API:

```javascript
Comm100API.do('livechat.chat.restore');
```

#### Close

You could listen to close event of chat panel:

```javascript
Comm100API.on('livechat.chat.close', function () { });
```

To close the chat panel manually, use following API:

```javascript
Comm100API.do('livechat.chat.close');
```

Please notice that closing chat will not end chat by default. To ensure the ongoing chat is ended before window is closed, you could use the API described below.

#### End Chat

To end ongoing chat, use following API:

```javascript
Comm100API.do('livechat.chat.endChat');
```

#### Replacement

There are several components in chat panel which can be replaced by your own component, following are they and how you could replace:

**Header Icons**

```javascript
Comm100API.render('livechat.chat.header.icons', function () {
  // return custom HTMLElement to replace default component
});
```

**Header**

```javascript
Comm100API.render('livechat.chat.header', function () {
  // return custom HTMLElement to replace default component
});
```

**Footer**

```javascript
Comm100API.render('livechat.chat.footer', function () {
  // return custom HTMLElement to replace default component
});
```

**Powered-by Area**

```javascript
Comm100API.render('livechat.chat.footer.poweredby', function () {
  // return custom HTMLElement to replace default component
});
```

#### Audio/Video Chat

To initiate audio chat, use following API:

```javascript
/** @type {Promise<string>} */
const promise = Comm100API.do('livechat.chat.audio.request');
```

To initiate video chat, use following API:

```javascript
/** @type {Promise<string>} */
const promise = Comm100API.do('livechat.chat.video.request');
```

Calling this API is same as clicking audio/video icon on chat window. That means, it might fail if audio/video chat is not available. The returned promise will reject with error string when failed. Following are possible error strings rejected by promise:

* `webrtc_not_supported`: brorwser of either agent or visitor does not support WebRTC.
* `audio_video_chatting`: visitor is either audio chatting or video chatting.
* `frequency_limited`: number of messages sent out by visitor during a certain period of time has reached upper limitation, cannot send out further message at the moment.
* `audio_not_enabled`: audio chat is not enabled in this campaign
* `video_not_enabled`: video chat is not enabled in this campaign
* `not_chatting`: visitor is currently not chatting

Following are available events that can be listened to when audio/video chat status changed:

Request of Audio/Video Chat:

```javascript
Comm100API.on('livechat.chat.audio.request', function () { });
Comm100API.on('livechat.chat.video.request', function () { });
```

Refuse of Audio/Video Chat:

```javascript
Comm100API.on('livechat.chat.audio.refuse', function () { });
Comm100API.on('livechat.chat.video.refuse', function () { });
```

Start of Audio/Video Chat:

```javascript
Comm100API.on('livechat.chat.audio.start', function () { });
Comm100API.on('livechat.chat.video.start', function () { });
```

Stop of Audio/Video Chat:

```javascript
Comm100API.on('livechat.chat.audio.stop', function () { });
Comm100API.on('livechat.chat.video.stop', function () { });
```

Cancel of Audio/Video Chat request:

```javascript
Comm100API.on('livechat.chat.audio.cancel', function () { });
Comm100API.on('livechat.chat.video.cancel', function () { });
```

Audio/Video Chat request has not been answered:

```javascript
Comm100API.on('livechat.chat.audio.noanswer', function () { });
Comm100API.on('livechat.chat.video.noanswer', function () { });
```

### Post-Chat

#### Fields

**Get All Fields**

You could get all fields in postchat using API:

```javascript
/** @type {string} */
const campaignId = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx';
/** @type {Array<Field>} */
const fields = Comm100API.get('livechat.postChat.fields', campaignId);
```

Campaign ID is optional parameter that can be used to determine which campaign you would like to get. If it is not provided, the first campaign ID returned by `Comm100API.get('livechat.campaignIds')` will be used as default value.

Here, the API will return a list of fields, each is an object representing the field in form, which has same structure as shown in [PreChat](broken-reference).

**Set All Fields**

Similar as getting all fields for postchat, you could also use API to set all fields for postchat, including it's value, error messages, etc.:

```javascript
/** @type {string} */
const campaignId = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx';
/** @type {Array<Field>} */
const fields = [];
Comm100API.set('livechat.postChat.fields', fields, campaignId);
```

Campaign ID is optional parameter that can be used to determine which campaign you would like to get. If it is not provided, the first campaign ID returned by `Comm100API.get('livechat.campaignIds')` will be used as default value.

Besides all types of fields listed above, you could also set custom field if system provided types cannot fulfill all your requirement:

```javascript
const fields = [{
  type: 'custom', // type of field
  element: function (update) {
    const component = document.createElement('input');
    component.onchange = function (event) {
      const value = event.target.value;
      update(value);
    };
    return component;
  },
}];
Comm100API.set('livechat.postChat.fields', fields);
```

In example above, a custom field is defined.

The attribute `element` provides a callback function which will be used to render element. It should return an HTMLElement which will be placed in form. When callback is triggered, `update` as a function will be provided. Custom component should use this `update` function to inform the system whenever the value of field has been changed: `update(value);`.

**Field Change Event**

To listen to change event of fields, use following API:

```javascript
Comm100API.on('livechat.postChat.field.change', function (field) { });
```

`field` will be provided when event fires, the structure of this object can be found [here](broken-reference).

**Blur and Focus**

To focus on specific field, use following API:

```javascript
/** @type {string} */
const fieldId = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx';
Comm100API.do('livechat.postChat.field.focus', fieldId);
```

To listen to focus/blur event, use one of the following APIs:

```javascript
Comm100API.on('livechat.postChat.field.focus', function (field) { });
Comm100API.on('livechat.postChat.field.blur', function (field) { });
```

`field` will be provided when event fires, the structure of this object can be found [here](broken-reference).

#### Submit Form

**Event**

When visitor submit postchat form, corresponding event will be triggered. Following API shows how to listen to this event:

```javascript
Comm100API.on('livechat.postChat.submit', function (formData) { });
```

Here, `formData` is an object containing all values of fields in postchat form. The structure is the same as described in [PreChat](broken-reference).

This event will be triggered when visitor submits the form.

When submitting is succeed, following event will be triggered:

```javascript
Comm100API.on('livechat.postChat.submit.success', function () { });
```

However, if server reject it, following event will be triggered:

```javascript
Comm100API.on('livechat.postChat.submit.fail', function (message) { });
```

where `message` is a string indicating the error message.

**Submit Action**

You could also use API to submit postchat form directly:

```javascript
/** @type {Promise<void>} */
const promise = Comm100API.do('livechat.postChat.submit');
```

The chat window requires to be opened when submitting the form. Calling this API is equivalent to clicking the submit button by user. That means, if any of the fields has issue, submit will fail. Hence, for this API, a promise will be returned, indicating whether submit success or not. If promise is rejected, a list of IDs will be provided as first parameter, indicating which fields are invalid due to incorrect values; if first parameter is `null`, you could check the second parameter for possible error message.

#### Display

You could listen to display event of postchat form:

```javascript
Comm100API.on('livechat.postChat.display', function () { });
```

#### Minimize

You could listen to minimize event of postchat form:

```javascript
Comm100API.on('livechat.postChat.minimize', function () { });
```

To minimize the form manually, use following API:

```javascript
Comm100API.do('livechat.postChat.minimize');
```

#### Restore

You could listen to restore event of postchat form:

```javascript
Comm100API.on('livechat.postChat.restore', function () { });
```

To restore the form manually, use following API:

```javascript
Comm100API.do('livechat.postChat.restore');
```

#### Close

You could listen to close event of postchat form:

```javascript
Comm100API.on('livechat.postChat.close', function () { });
```

To close the form manually, use following API:

```javascript
Comm100API.do('livechat.postChat.close');
```

#### Replacement

There are several components in postchat form which can be replaced by your own component, following are they and how you could replace:

**Header Icons**

```javascript
Comm100API.render('livechat.postChat.header.icons', function () {
  // return custom HTMLElement to replace default component
});
```

**Header**

```javascript
Comm100API.render('livechat.postChat.header', function () {
  // return custom HTMLElement to replace default component
});
```

**Greeting Message Area**

```javascript
Comm100API.render('livechat.postChat.greeting', function () {
  // return custom HTMLElement to replace default component
});
```

**Postchat Form**

```javascript
Comm100API.render('livechat.postChat.form', function () {
  // return custom HTMLElement to replace default component
});
```

**Submit Button**

```javascript
Comm100API.render('livechat.postChat.submit', function () {
  // return custom HTMLElement to replace default component
});
```

**Footer**

```javascript
Comm100API.render('livechat.postChat.footer', function () {
  // return custom HTMLElement to replace default component
});
```

**Powered-by Area**

```javascript
Comm100API.render('livechat.postChat.footer.poweredby', function () {
  // return custom HTMLElement to replace default component
});
```

### Offline Message

#### Fields

**Get All Fields**

You could get all fields in Offline Message using API:

```javascript
/** @type {string} */
const campaignId = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx';
/** @type {Array<Field>} */
const fields = Comm100API.get('livechat.offlineMessage.fields', campaignId);
```

Campaign ID is optional parameter that can be used to determine which campaign you would like to get. If it is not provided, the first campaign ID returned by `Comm100API.get('livechat.campaignIds')` will be used as default value.

Here, the API will return a list of fields, each is an object representing the field in form, which has same structure as shown in [PreChat](broken-reference).

**Set All Fields**

Similar as getting all fields for Offline Message, you could also use API to set all fields for Offline Message, including it's value, error messages, etc.:

```javascript
/** @type {string} */
const campaignId = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx';
/** @type {Array<Field>} */
const fields = [];
Comm100API.set('livechat.offlineMessage.fields', fields, campaignId);
```

Campaign ID is optional parameter that can be used to determine which campaign you would like to get. If it is not provided, the first campaign ID returned by `Comm100API.get('livechat.campaignIds')` will be used as default value.

Besides all types of fields listed above, you could also set custom field if system provided types cannot fulfill all your requirement:

```javascript
const fields = [{
  type: 'custom', // type of field
  element: function (update) {
    const component = document.createElement('input');
    component.onchange = function (event) {
      const value = event.target.value;
      update(value);
    };
    return component;
  },
}];
Comm100API.set('livechat.offlineMessage.fields', fields);
```

In example above, a custom field is defined.

The attribute `element` provides a callback function which will be used to render element. It should return an HTMLElement which will be placed in form. When callback is triggered, `update` as a function will be provided. Custom component should use this `update` function to inform the system whenever the value of field has been changed: `update(value);`.

**Field Change Event**

To listen to change event of fields, use following API:

```javascript
Comm100API.on('livechat.offlineMessage.field.change', function (field) { });
```

`field` will be provided when event fires, the structure of this object can be found [here](broken-reference).

**Blur and Focus**

To focus on specific field, use following API:

```javascript
/** @type {string} */
const fieldId = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx';
Comm100API.do('livechat.offlineMessage.field.focus', fieldId);
```

To listen to focus/blur event, use one of the following APIs:

```javascript
Comm100API.on('livechat.offlineMessage.field.focus', function (field) { });
Comm100API.on('livechat.offlineMessage.field.blur', function (field) { });
```

`field` will be provided when event fires, the structure of this object can be found [here](broken-reference).

#### Submit Form

**Event**

When visitor submit Offline Message form, corresponding event will be triggered. Following API shows how to listen to this event:

```javascript
Comm100API.on('livechat.offlineMessage.submit', function (formData) { });
```

Here, `formData` is an object containing all values of fields in Offline Message form. The structure is the same as described in [PreChat](broken-reference).

This event will be triggered when visitor submits the form.

When submitting is succeed, following event will be triggered:

```javascript
Comm100API.on('livechat.offlineMessage.submit.success', function () { });
```

However, if server reject it, following event will be triggered:

```javascript
Comm100API.on('livechat.offlineMessage.submit.fail', function (message) { });
```

where `message` is a string indicating the error message.

**Submit Action**

You could also use API to submit Offline Message form directly:

```javascript
/** @type {Promise<void>} */
const promise = Comm100API.do('livechat.offlineMessage.submit');
```

The chat window requires to be opened when submitting the form. Calling this API is equivalent to clicking the submit button by user. That means, if any of the fields has issue, submit will fail. Hence, for this API, a promise will be returned, indicating whether submit success or not. If promise is rejected, a list of IDs will be provided as first parameter, indicating which fields are invalid due to incorrect values; if first parameter is `null`, you could check the second parameter for possible error message.

#### Display

You could listen to display event of Offline Message form:

```javascript
Comm100API.on('livechat.offlineMessage.display', function () { });
```

#### Minimize

You could listen to minimize event of Offline Message form:

```javascript
Comm100API.on('livechat.offlineMessage.minimize', function () { });
```

To minimize the form manually, use following API:

```javascript
Comm100API.do('livechat.offlineMessage.minimize');
```

#### Restore

You could listen to restore event of Offline Message form:

```javascript
Comm100API.on('livechat.offlineMessage.restore', function () { });
```

To restore the form manually, use following API:

```javascript
Comm100API.do('livechat.offlineMessage.restore');
```

#### Close

You could listen to close event of Offline Message form:

```javascript
Comm100API.on('livechat.offlineMessage.close', function () { });
```

To close the form manually, use following API:

```javascript
Comm100API.do('livechat.offlineMessage.close');
```

#### Replacement

There are several components in Offline Message form which can be replaced by your own component, following are they and how you could replace:

**Header Icons**

```javascript
Comm100API.render('livechat.offlineMessage.header.icons', function () {
  // return custom HTMLElement to replace default component
});
```

**Header**

```javascript
Comm100API.render('livechat.offlineMessage.header', function () {
  // return custom HTMLElement to replace default component
});
```

**Greeting Message Area**

```javascript
Comm100API.render('livechat.offlineMessage.greeting', function () {
  // return custom HTMLElement to replace default component
});
```

**Postchat Form**

```javascript
Comm100API.render('livechat.offlineMessage.form', function () {
  // return custom HTMLElement to replace default component
});
```

**Submit Button**

```javascript
Comm100API.render('livechat.offlineMessage.submit', function () {
  // return custom HTMLElement to replace default component
});
```

**Footer**

```javascript
Comm100API.render('livechat.offlineMessage.footer', function () {
  // return custom HTMLElement to replace default component
});
```

**Powered-by Area**

```javascript
Comm100API.render('livechat.offlineMessage.footer.poweredby', function () {
  // return custom HTMLElement to replace default component
});
```

### Side Window

#### Display

To see if side window is visible, you could use the following API:

```javascript
/** @type {boolean} */
const isVisible = Comm100API.get('livechat.sideWindow.isVisible');
```

To set the visibility of side window, you could use the following API:

```javascript
/** @type {boolean} */
const isVisible = true;
Comm100API.set('livechat.sideWindow.isVisible', isVisible);
```

If you are interested in the display event of side window, you could use one of the following APIs:

```javascript
Comm100API.on('livechat.sideWindow.show', function () { });
Comm100API.on('livechat.sideWindow.hide', function () { });
```

#### Tab

To get all available tabs of side window, you could use API:

```javascript
/** @type {Array<Tab>} */
const tabs = Comm100API.get('livechat.sideWindow.tabs');
```

Here, the API will return a list of objects representing available tabs, where each has one of the following structures:

* System tab

```javascript
const tab = {
  id: '', // string, id of tab
  isSystem: true, // boolean, whether tab is from system
  'aria-label': '', // aria-label that will be used for this tab icon
};
```

* Custom tab

```javascript
const tab = {
  id: '', // string, id of  tab
  isSystem: false, // boolean
  content: function () {
    const component = document.createElment('div');
    return component;
  },
  iconImg: '', // string, url of image, which will be used to display icon in tab
  'aria-label': '', // aria-label that will be used for this icon
}
```

Here, `content` should be a callback function to generate custom HTMLElement.

You could also use the following API to set all tabs:

```javascript
const tabs = [];
Comm100API.set('livechat.sideWindow.tabs', tabs);
```

To listen to tab switch events, you could use the following API:

```javascript
Comm100API.on('livechat.sideWindow.tabs.switch', function(currentTabId) {});
```

Selected tab id will be provided when callback triggered.

### SSO (Single Sign On)

#### Artifact

**Get**

To get the artifact you set in, you could use the following API:

```javascript
const artifact = Comm100API.get('livechat.sso.artifact');
```

**Set**

To set artifact for sso login, you could use the following API:

```javascript
const artifact = 'artifact here';
Comm100API.set('livechat.sso.artifact', artifact);
```
