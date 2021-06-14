Java Generics - a problem I couldn't quite overcome
===============

Posted: 2010-05-06 10:48:32
-------------------------

For the most part, Java Generics work quite well for 99% of my needs when doing generic programming. The API is fairly intuitive, and I rarely have to use the weird features like <code><? super T></code> or whatever. However, I came across a situation that generics couldn't quite handle.

I was working on a GWT project, and I found myself looking for a sortable table much like a JTable can do pretty much out of the box in Swing. I found a simple implementation that stores each row of the table in a RowData class. Now, the thing about such a row is that an instance of a row would contain several different objects of varying types; maybe a string in the first column,&#160; a few numbers in the next columns, a date or two in other columns. So we can't parameterize the RowData class directly... or can we? We do want to specify that the types of objects we put in our RowData instances are instances of Comparable, because we are going to sort a collection of rows on one column (thus, the RowData itself is declared as <code>public class RowData implements Comparable<RowData></code>). So would it make sense to declare the class as <code>public class RowData<T extends Comparable<? super T>> ...</code>? No, because then you are restricting the type of object you are using in the RowData. We have to leave it unparameterized.

So we have to maintain a list of these objects. Well, maybe we can just parameterize this list as a list of Comparables, since we know they all have to be Comparable:

```java
  private List<Comparable<?>>
  columnValues = new ArrayList<Comparable<?>>();
```

Seems sensible. However, we are going to have a problem in our <code>compareTo()</code> method:

```java
  public int compareTo(RowData other) {
		Comparable<?> obj1 = this.getColumnValue(this.sortColIndex);
		Comparable<?> obj2 =  other.getColumnValue(this.sortColIndex);

		return obj1.compareTo(obj2); // compiler error!
	}
```

In the end, I had to compromise and drop the used of generics. My coworker Andy called this solution "concise, but not precise."

```
@SuppressWarnings("unchecked")
public int compareTo(RowData other) {
	if (null == other) {
		return -1;
	}

	if (!(this.getColumnValue(this.sortColIndex) instanceof Comparable<?>)
		|| !(other.getColumnValue(this.sortColIndex) instanceof Comparable<?>)) {
		return 0;
	}
		
	// I'm promising that these are mutually comparable, but guaranteeing this
	// at compile-time is nigh impossible!
		
	Comparable obj1 = (Comparable) this.getColumnValue(this.sortColIndex);
	Comparable obj2 = (Comparable) other.getColumnValue(this.sortColIndex);
		
	return obj1.compareTo(obj2);
}
```

Inelegant, but I couldn't come up with a better way to do it. I'm thinking you could use reflection to cast the objects, but unfortunately GWT does not allow much access to the reflection API beyond <code>getClass()</code> and the <code>instanceof</code> operator.
