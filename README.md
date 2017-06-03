# Codeperlen

> face.apply(palm)

A collection of strange code fragments I stumbled upon in reviews

Have fun :)

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

Placing duplicates of this method in many other classes wasn't a good idea as well.

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


## Trying to be fancy

Regular expressions are tremendously cool, *but* be sure to use them the right way.

...And write `fsck`ing Unit tests for your methods!

```java
public boolean isValid(String birthYear, ConstraintValidatorContext context) {

	if (value != null && value.matches("\\d{4}")) {
		int birthYear = Integer.valueOf(birthYear);
		int currentYear = Calendar.getInstance().get(Calendar.YEAR);
		int before100Years = currentYear - 100;

		if (birthYear > currentYear || birthYear < before100Years) {
			return false;
		}
	}

	return true;
}
```


Valid years (according to this method) are:

`KITTY`, `10000`, `500`, `-2000`, `1000 a.d.`
