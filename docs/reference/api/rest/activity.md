---
pageClass: page-reference
---

# Activity

<div class="two-up">
<div class="left">

All events that happen within Directus are tracked and stored in the activities collection. This gives you full
accountability over everything that happens.

</div>
<div class="right">

[[toc]]

</div>
</div>

---

## The Activity Object

<div class="two-up">
<div class="left">
<div class="definitions">

`action` **string**\
Action that was performed.

`collection` **string**\
Collection identifier in which the item resides.

`comment` **string**\
User comment. This will store the comments that show up in the right sidebar of the item edit page in the admin app.

`id` **integer**\
Unique identifier for the object.

`ip` **string**\
The IP address of the user at the time the action took place.

`item` **string**\
Unique identifier for the item the action applied to. This is always a string, even for integer primary keys.

`timestamp` **string**\
When the action happened.

`user` **many-to-one**\
The user who performed this action. Many-to-one to [users](/reference/api/rest/users/#the-users-object).

`user_agent` **string**\
User agent string of the browser the user used when the action took place.

`revisions` **one-to-many**\
Any changes that were made in this activity. One-to-many to [revisions](/reference/api/rest/revisions/#the-revisions-object).

</div>
</div>
<div class="right">

```json
{
	"action": "create",
	"collection": "articles",
	"comment": null,
	"id": 5,
	"ip": "139.178.128.0",
	"item": "1",
	"timestamp": "2021-02-02T12:50:26-05:00",
	"user": "2d321940-69f5-445f-be6b-c773fa58a820",
	"user_agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_6) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/14.0.2 Safari/605.1.15",
	"revisions": [4]
}
```

</div>
</div>

---

## List Activity Actions

Returns a list of activity actions.

<div class="two-up">
<div class="left">

### Query Parameters

Supports all [global query parameters](/reference/api/query).

### Returns

An array of up to [limit](/reference/api/query/#limit) [activity objects](#the-activity-object). If no items are
available, data will be an empty array.

</div>
<div class="right">

### `GET /activity`

```json
{
	"data": [
		{
			"id": 1,
			"action": "create",
			"user": "0bc7b36a-9ba9-4ce0-83f0-0a526f354e07",
			"timestamp": "2021-01-27T10:14:33-05:00",
			"ip": "127.0.0.1",
			"user_agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 11_1_0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.88 Safari/537.36 Edg/87.0.664.60",
			"collection": "directus_fields",
			"item": "1",
			"comment": null,
			"revisions": [2]
		},
		{...},
		{...}
	]
}
```

</div>
</div>

---

## Retrieve Activity Action

Returns a single activity action by primary key.

<div class="two-up">
<div class="left">

### Query Parameters

Supports all [global query parameters](/reference/api/query).

### Returns

Returns an [activity object](#the-activity-object) if a valid identifier was provided.

</div>
<div class="right">

### `GET /activity/:id`

```json
{
	"data": {
		"id": 1,
		"action": "create",
		"user": "0bc7b36a-9ba9-4ce0-83f0-0a526f354e07",
		"timestamp": "2021-01-27T10:14:33-05:00",
		"ip": "127.0.0.1",
		"user_agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 11_1_0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.88 Safari/537.36 Edg/87.0.664.60",
		"collection": "directus_fields",
		"item": "1",
		"comment": null,
		"revisions": [2]
	}
}
```

</div>
</div>

---

## Create a Comment

Creates a new comment on a given item.

<div class="two-up">
<div class="left">

### Request Body

<div class="definitions">

`collection` **Required**\
Collection in which the item resides.

`item` **Required**\
Primary Key of the item to comment on.

`comment` **Required**\
The comment content. Supports Markdown.

</div>

### Returns

Returns the [activity object](#the-activity-object) of the created comment.

</div>
<div class="right">

### `POST /activity/comment`

```json
// Request

{
	"collection": "pages",
	"item": 3,
	"comment": "Hello World"
}
```

```json
// Response

{
	"data": {
		"id": 390,
		"action": "comment",
		"user": "0bc7b36a-9ba9-4ce0-83f0-0a526f354e07",
		"timestamp": "2021-02-03T18:04:32-05:00",
		"ip": "127.0.0.1",
		"user_agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 11_1_0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.88 Safari/537.36 Edg/87.0.664.60",
		"collection": "pages",
		"item": "3",
		"comment": "Hello World",
		"revisions": null
	}
}
```

</div>
</div>

---

## Update a Comment

Updates an existing comment by activity action primary key.

<div class="two-up">
<div class="left">

### Request Body

<div class="definitions">

`comment` **Required**\
The updated comment content. Supports Markdown.

</div>

### Returns

Returns the [activity object](#the-activity-object) of the created comment.

</div>
<div class="right">

### `PATCH /activity/comment/:id`

```json
// Request

{
	"comment": "Hello World!!"
}
```

```json
// Response

{
	"data": {
		"id": 390,
		"action": "comment",
		"user": "0bc7b36a-9ba9-4ce0-83f0-0a526f354e07",
		"timestamp": "2021-02-03T18:04:32-05:00",
		"ip": "127.0.0.1",
		"user_agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 11_1_0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.88 Safari/537.36 Edg/87.0.664.60",
		"collection": "pages",
		"item": "3",
		"comment": "Hello World!!",
		"revisions": null
	}
}
```

</div>
</div>

---

## Delete a Comment

Deletes a comment.

<div class="two-up">
<div class="left"></div>
<div class="right">

### `DELETE /activity/comment/:id`

```json
// Empty Response
```

</div>
</div>
