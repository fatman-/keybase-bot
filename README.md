# keybase-bot

[![npm](https://img.shields.io/npm/v/keybase-bot.svg)](https://www.npmjs.com/package/keybase-bot)

Script Keybase functionality in Node.js!

This module is a side-project/work in progress and may change or have crashers, but feel free to play with it. As long as you have a Keybase account and a paper key, you can use this module to script basic Keybase commands.

## Installation

1.  Install Node.js 8 or above. You can do this [directly from the Node.js website](https://nodejs.org/en/download) or [via your favorite package manager](https://nodejs.org/en/download/package-manager/).
2.  Make sure that you have Keybase [installed](https://keybase.io/download) and running.
3.  Install `keybase-bot`. You can do this using either [npm](https://www.npmjs.com) or [Yarn](https://yarnpkg.com).
    ```bash
    npm install keybase-bot
    # or
    yarn add keybase-bot
    ```

You're ready to make your first Keybase bot!

## Hello world via your Keybase bot

Let's make a bot that says hello to the Keybase user [kbot](https://keybase.io/kbot).

```javascript
const Bot = require('keybase-bot')

const bot = new Bot()

try {
  await bot.init({username: process.env.KB_USERNAME, paperkey: process.env.KB_PAPERKEY, verbose: false})
  console.log(`Your bot is initialized. It is logged in as ${bot.myInfo().username}`)
  const channel = {name: 'kbot,' + bot.myInfo().username, public: false, topic_type: 'chat'}
  const sendArg = {
    channel: channel,
    message: {
      body: `Hello kbot! This is ${bot.myInfo().username} saying hello from my device ${
        bot.myInfo().devicename
      }`,
    },
  }
  await bot.chatSend(sendArg)
  console.log('Message sent!')
} catch (error) {
  console.error(error)
} finally {
  await bot.deinit()
}
```

This code is also in [`demos/hello_world.js`](demos/hello_world.js), if you want to take a look in there. There are also some other cool bots in the demos directory, including a bot that tells you how many unread messages you have and a bot that does math for you and your friends.

To run the above bot, you want to save that code into a file and run it with node:

```bash
node <my-awesome-file-name>.js
```

## API

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

#### Table of Contents

- [Bot](#bot)
  - [init](#init)
    - [Parameters](#parameters)
  - [deinit](#deinit)
  - [myInfo](#myinfo)
  - [chatList](#chatlist)
    - [Parameters](#parameters-1)
  - [chatSend](#chatsend)
    - [Parameters](#parameters-2)
  - [chatRead](#chatread)
    - [Parameters](#parameters-3)
  - [chatDelete](#chatdelete)
    - [Parameters](#parameters-4)
  - [watchChannelForNewMessages](#watchchannelfornewmessages)
    - [Parameters](#parameters-5)
  - [watchAllChannelsForNewMessages](#watchallchannelsfornewmessages)
    - [Parameters](#parameters-6)
    - [Examples](#examples)
- [Types](#types)
  - [AllChatOptions](#allchatoptions)
  - [InitOptions](#initoptions)
    - [Properties](#properties)
  - [ChatOptionsList](#chatoptionslist)
    - [Properties](#properties-1)
  - [ChatOptionsSend](#chatoptionssend)
    - [Properties](#properties-2)
  - [ChatOptionsRead](#chatoptionsread)
    - [Properties](#properties-3)
  - [ChatOptionsEdit](#chatoptionsedit)
    - [Properties](#properties-4)
  - [ChatOptionsReaction](#chatoptionsreaction)
    - [Properties](#properties-5)
  - [ChatOptionsDelete](#chatoptionsdelete)
    - [Properties](#properties-6)
  - [ChatOptionsAttachment](#chatoptionsattachment)
    - [Properties](#properties-7)
  - [ChatOptionsDownload](#chatoptionsdownload)
    - [Properties](#properties-8)
  - [ChatOptionsSetStatus](#chatoptionssetstatus)
    - [Properties](#properties-9)
  - [ChatOptionsMark](#chatoptionsmark)
    - [Properties](#properties-10)
  - [ChatOptionsSearchInbox](#chatoptionssearchinbox)
  - [ChatOptionsSearchRegexp](#chatoptionssearchregexp)
  - [ChatReadMessage](#chatreadmessage)
  - [DeviceUsernamePair](#deviceusernamepair)
    - [Properties](#properties-11)
- [UsernameAndDevice](#usernameanddevice)
  - [Properties](#properties-12)

### Bot

A Keybase bot.

#### init

`bot.init()` must be run to initialize the bot before using other methods. It
checks to make sure you're properly logged into Keybase and gets basic
info about your session. Afterwards, feel free to check bot.myInfo() to
see or check who you're logged in as.

##### Parameters

- `options` **[InitOptions](#initoptions)**

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)&lt;[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?>**

#### deinit

Deinitializes a bot by logging it out of its current Keybase session.
Should be run after your bot finishes.

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)&lt;[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?>**

#### myInfo

### myInfo

Returns **[DeviceUsernamePair](#deviceusernamepair)?**

#### chatList

Lists your chats, with info on which ones have unread messages.

##### Parameters

- `options` **[ChatOptionsList](#chatoptionslist)**

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)&lt;any>**

#### chatSend

Reads the messages in a channel. You can read with or without marking as read.

##### Parameters

- `options` **[ChatOptionsSend](#chatoptionssend)**

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)&lt;any>**

#### chatRead

Sends a message to a certain channel.

##### Parameters

- `options` **[ChatOptionsRead](#chatoptionsread)**

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)&lt;any>**

#### chatDelete

Deletes a message in a channel. Messages have messageId's associated with
them, which you can learn in `bot.chatRead`. Known bug: the GUI has a cache,
and deleting from the CLI may not become apparent immediately.

##### Parameters

- `options` **[ChatOptionsDelete](#chatoptionsdelete)**

Returns **[Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)&lt;any>**

#### watchChannelForNewMessages

Listens for new chat messages on a specified channel. The `onMessage` function is called for every message your bot receives.
This is pretty similar to `watchAllChannelsForNewMessages`, except it specifically checks one channel.

##### Parameters

- `channel` **ChatChannel**
- `onMessage` **function (message: [ChatReadMessage](#chatreadmessage)): void**

#### watchAllChannelsForNewMessages

This function will put your bot into insane mode, where it reads
everything it can and every new message it finds it will pass to you, so
you can do what you want with it. For example, if you want to write a
Keybase bot that talks shit at anyone who dares approach it, this is the
function to use.\*

Specifically, it will call the `onMessage` function you provide for every
message your bot receives.\*

##### Parameters

- `onMessage` **function (message: [ChatReadMessage](#chatreadmessage)): void**

##### Examples

```javascript
// reply to incoming traffic on all channels with 'thanks!'
var onMessages = function(m) {
  var channel = m.channel
  var messages = m.messages // we could look in this array to read them and write custom replies
  bot.chatSend(
    {
      channel: channel,
      message: {
        body: 'thanks!!!',
      },
    },
    function(err, res) {
      if (err) {
        console.log(err)
      }
    }
  )
}
bot.watchAllChannelsForNewMessages({onMessages: onMessages})
```

### Types

Collection of options for chat api

#### AllChatOptions

Type: ([ChatOptionsList](#chatoptionslist) \| [ChatOptionsRead](#chatoptionsread) \| [ChatOptionsSend](#chatoptionssend) \| [ChatOptionsEdit](#chatoptionsedit) \| [ChatOptionsDelete](#chatoptionsdelete) \| [ChatOptionsSetStatus](#chatoptionssetstatus) \| [ChatOptionsMark](#chatoptionsmark) \| [ChatOptionsReaction](#chatoptionsreaction) \| [ChatOptionsSearchRegexp](#chatoptionssearchregexp) \| [ChatOptionsSearchInbox](#chatoptionssearchinbox) \| [ChatOptionsDownload](#chatoptionsdownload) \| [ChatOptionsAttachment](#chatoptionsattachment))

#### InitOptions

Type: {username: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String), paperkey: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String), verbose: [boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?}

##### Properties

- `username` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)**
- `paperkey` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)**
- `verbose` **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?**

#### ChatOptionsList

Type: {unreadOnly: [boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?, topicType: TopicType?, showErrors: [boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?, failOffline: [boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?}

##### Properties

- `unreadOnly` **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?**
- `topicType` **TopicType?**
- `showErrors` **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?**
- `failOffline` **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?**

#### ChatOptionsSend

Type: {channel: ChatChannel, conversationId: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?, message: ChatMessage, nonblock: [boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?, membersType: MembersType}

##### Properties

- `channel` **ChatChannel**
- `conversationId` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?**
- `message` **ChatMessage**
- `nonblock` **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?**
- `membersType` **MembersType**

#### ChatOptionsRead

Type: {channel: ChatChannel, conversationId: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?, peek: [boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean), pagination: Pagination?, unreadOnly: [boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean), failOffline: [boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)}

##### Properties

- `channel` **ChatChannel**
- `conversationId` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?**
- `peek` **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)**
- `pagination` **Pagination?**
- `unreadOnly` **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)**
- `failOffline` **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)**

#### ChatOptionsEdit

Type: {channel: ChatChannel, conversationId: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?, messageId: [number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number), message: ChatMessage}

##### Properties

- `channel` **ChatChannel**
- `conversationId` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?**
- `messageId` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)**
- `message` **ChatMessage**

#### ChatOptionsReaction

Type: {channel: ChatChannel, conversationId: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?, messageId: [number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number), message: ChatMessage}

##### Properties

- `channel` **ChatChannel**
- `conversationId` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?**
- `messageId` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)**
- `message` **ChatMessage**

#### ChatOptionsDelete

Type: {channel: ChatChannel, conversationId: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?, messageId: [number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)}

##### Properties

- `channel` **ChatChannel**
- `conversationId` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?**
- `messageId` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)**

#### ChatOptionsAttachment

Type: {channel: ChatChannel, conversationId: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?, filename: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String), title: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String), preview: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?}

##### Properties

- `channel` **ChatChannel**
- `conversationId` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?**
- `filename` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)**
- `title` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)**
- `preview` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?**

#### ChatOptionsDownload

Type: {channel: ChatChannel, conversationId: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?, messageId: [number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number), output: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String), preview: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?, noStream: [boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?}

##### Properties

- `channel` **ChatChannel**
- `conversationId` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?**
- `messageId` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)**
- `output` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)**
- `preview` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?**
- `noStream` **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)?**

#### ChatOptionsSetStatus

Type: {channel: ChatChannel, conversationId: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?, status: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)}

##### Properties

- `channel` **ChatChannel**
- `conversationId` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?**
- `status` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)**

#### ChatOptionsMark

Type: {channel: ChatChannel, conversationId: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?, messageId: [number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)}

##### Properties

- `channel` **ChatChannel**
- `conversationId` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?**
- `messageId` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)**

#### ChatOptionsSearchInbox

Type: any

#### ChatOptionsSearchRegexp

Type: any

#### ChatReadMessage

Type: any

#### DeviceUsernamePair

Type: {username: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String), devicename: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)}

##### Properties

- `username` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)**
- `devicename` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)**

### UsernameAndDevice

Type: {username: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String), devicename: [string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)}

#### Properties

- `username` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)**
- `devicename` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)**

## Contributions

We welcome contributions from you!

- please install dev dependencies and yarn (an improved npm)
- make sure this passes `yarn build` and `yarn flow`
- if adding a new feature, make a demo or something
