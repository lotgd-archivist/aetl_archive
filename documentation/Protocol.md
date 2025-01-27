# Protocol

## Basic request structure

Requests are made against the `aetl.php` of the respective game server.

All requests made from AETL are a POST Request with the following fields in the quest body:

- `login` - The username as provided in the login dialog.
- `pwhash` - the MD5 hash of the users password.

Some requests also contain a `data` section with an additional payload. Those are specified under "Data" below, where relevant.

## Requests

### setread
**Description:** Sets one or more mails (via their ID) to "read".  
**Query parameter:**: `?op=setread`  
**Data:**
```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<aetl>
	<taube id="{messageId1}" />
	<taube id="{messageId2}" />
	<!-- ... -->
</aetl>
```

Each `<taube>`  element specifies one message to mark as read.

### setunread
**Description:** Sets one or more mails (via their ID) to "unread".  
**Query parameter:**: `?op=setunread`  
**Data:**
```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<aetl>
	<taube id="{messageId1}" />
	<taube id="{messageId2}" />
	<!-- ... -->
</aetl>
```

Each `<taube>`  element specifies one message to mark as unread.

### checkout
**Description:** Finds the account ID for a target username/login.  
**Query parameter:**: `?op=checkout&putwhat=login`  
**Data:** The username to find the account ID for.

### login
**Description:** Checks the credentials for validity.  
**Query parameter:** `?op=login`  

### getmails
**Description:** Returns the current inbox of the user.  
**Query parameter:** `?op=getmails`  

### sendmail
**Description:** Send an ingame mail.  
**Query parameter:**: `?op=sendmail`  
**Request body:**
```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<aetl>
	<taube>
		<header>
			<to>{recipientId}</to>
			<subject>
				<![CDATA[{subject}]]>
			</subject>
			<answerid>{replyId}</answerid>
		</header>
		<body>
			<![CDATA[{body}]]>
		</body>
	</taube>
</aetl>
```

The XML describes the bare minimum details to send the mail from the users account. 

`aetl/taube/header/to` contains the recipients account ID, obtained via the `checkout` API call.

`aetl/taube/header/subject` is the user provided subject line in a CDATA section. 

`aetl/taube/header/answerid` is either the ID of the message the user has written a reply to, or 0 if the message is an entirely
new message.

`aetl/taube/body` contains the user provided mail text in a CDATA section.