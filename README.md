## Introduction

With the introduction of the new [Telegram Bot Platform](https://core.telegram.org/bots), you can now automate whatever task you want done inside Telegram.

Previously, you could create bots but it was less convenient, as you would need to provide a phone number and, in general, act as a normal Telegram account. This new API fixes some of these shortcomings.

With the new API, you can create a bot without having to provide a phone number.

When you register your bot via [GodfatherBot](https://core.telegram.org/bots#botfather), you will get your *api key* that you can then use to query `https://api.telegram.org/bot<apikey>`.

This url accepts different *method* calls.

For example, the `getMe` functions returns a JSON array with some basic function about your bot.

Typing...

```text
$ curl -i -X GET https://api.telegram.org/bot<apikey>/getMe
```

... in your terminal will get you:

```text
HTTP/1.1 200 OK
Server: nginx/1.9.1
Date: Sat, 04 Jul 2015 16:42:14 GMT
Content-Type: application/json
Content-Length: 87
Connection: keep-alive
Strict-Transport-Security: max-age=31536000; includeSubdomains

{"ok":true,"result":{"id":<bot-id>,"first_name":<bot-first-name>,"username":<bot-username>}}
```

It works!

## Diving in

There are a lot of different calls in the API, which you can see in the [API docs](https://core.telegram.org/bots/api), but we'll focus on the `getUpdates` and `sendMessage` methods here.

### getUpdates

Basically, anytime something happens that involves your bot, Telegram will place an `Update` object in your bot *update queue*. Objects in this queue will usually be stored up to 24 hours before Telegram deletes them.

You can then retrieve this queue by calling

```text
$ curl -i -X GET https://api.telegram.org/bot<apikey>/getUpdates
```

Among other things, Telegram will queue an `Update` object when:

- An user messages your bot, either directly or in a group.
- Your bot gets either added or kicked from a group.
- Someone gets either added or kicked from a group which contains your bot.
- Someone changes the title of a group which contains your bot.
- Someone changes the picture of a group which contains your bot.
- ...

This `Update` object will contain some info about that specific event that caused it to get queued.

For example, when someone messages your bot, calling your bot url from the terminal will get you:

```javascript
{
	"ok": Boolean,
	"result": [{
		"update_id": Integer,
		"message": {
			"message_id": Integer,
			"from": {
				"id": Integer,
				"first_name": String,
				"last_name": String,
				"username": String
			},
			"chat": {
				"id": Integer,
				"first_name": String,
				"last_name": String,
				"username": String
			},
			"date": Integer,
			"text": String
		}
	}, ...]
}
```

I've prettified the output, but you get the idea.

### sendMessage

Now that you maybe have a couple of message queued for your bot, it's time to reply them.

All Telegram API calls accepts either a **GET** with some query parameters or a **POST** with some *application/x-www-form-urlencoded* or *multipart/form-data* parameters.

For now, we are going to use a **GET** with some query parameters.

So, to reply to some queued message, simply type from your terminal...

```text
$ curl -i -X GET https://api.telegram.org/bot<apikey>/sendMessage?chat_id=<chatId>&text=<someText>
```

... to send the `<someText>` message to the `<chatId>` chat. The latter can either be a group id or a  normal chat id.

## Conclusion

Now that we know how to retrieve `Updates` from the Telegram server and how to reply to a specific user message, you can start writing your first bot.

Head over to the [API docs](https://core.telegram.org/bots/api) to learn more about different method calls and `Update` types.
