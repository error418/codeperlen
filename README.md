# Codeperlen

> face.apply(palm)

A collection of strange code fragments I stumbled upon in reviews


## Are you yourself?

```java
if (sortierung2[0] == null || sortierung2[0].equals(sortierung2[0])) {
	comparator = new RegistryBasedMultiComparator(sortInfo);
} else {
	[…]
}
```


## Everybody first!

The final modifier should have been extra-suspicious to the developer. Not to mention that the code is not working as intended (hint: there were no unit tests).

```java
public static String getMessageTypesAsJsonArray() {
final StringBuilder bob = new StringBuilder( "[" );
	final boolean first = true;
	for ( final MessageType type : MessageType.allValues() ) {
		if ( !first ) {
			bob.append( "," );
		}
		bob.append( "'" );
		bob.append( type.name() );
		bob.append( "'" );
	}
	bob.append( "]" );

	return bob.toString();
}
```


## Going the extra mile.. the wrong way

Violating the DRY-approach and repeating this method in many other classes wasn't a good idea as well.

```java
private String buildStringParamValueFromRequest() {
		Map<String, String> paramMap = ((ExternalContext) new Object()).getRequestParameterMap();

	if (!paramMap.isEmpty()) {
		StringBuilder sb = new StringBuilder("?");

        for (Entry<String, String> entry : paramMap.entrySet()) {
            sb.append(entry.getKey());
            sb.append("=");
            sb.append(entry.getValue());
            sb.append("&");
        }

		sb.setLength(sb.length() - 1);

		return sb.toString();
	}
	
    return "";
}
```

The more lazy (efficient) way:


```java
private String buildStringParamValueFromRequest() {
	HttpServletRequest request = (HttpServletRequest)facesContext.getExternalContext().getRequest();
	String query = request.getQueryString();
	if(query == null) {
		return StringUtils.EMPTY;
	}
	return "?" + query;
}

```
