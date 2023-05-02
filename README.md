Download Link: https://assignmentchef.com/product/solved-cot5930-homework3-unit-testing
<br>
<h1>Problem 1. Theory</h1>

<ol>

 <li>Explain how a lazy val can be used to memoize the value of an expression passed as by-name parameter.</li>

 <li>Extra credit 3 points * Extra credit 3 points * Extra credit: 3 points</li>

</ol>

Describe how the memoization and evaluation mechanism for a lazy val could be implemented by the Scala compiler.

<ol>

 <li>Consider method foldRight from the textbook Stream trait:</li>

</ol>

<strong>def</strong> foldRight[<strong>B</strong>](z<strong>:</strong> =&gt; B)(f<strong>:</strong> (<strong>A</strong>, =&gt; B) <strong>=&gt;</strong> B)<strong>:</strong> <strong>B</strong> =

<strong>this</strong> <strong>match</strong> {

<strong>case</strong> <strong>Cons</strong>(h,t) <strong>=&gt;</strong> f(h(), t().foldRight(z)(f))

<strong>case</strong> <strong>_</strong> <strong>=&gt;</strong> z     }

Explain under what condition(s) will expressions (or functions) using <em>foldRight</em> stop traversing the stream before reaching its end in contrast to traversing the entire stream unconditionally. Give a code example NOT covered in class with a function or expression using <em>foldRight </em>with incremental computation that stops mid-stream and another example NOT covered in class where the entire stream is always evaluated.

<ol start="5">

 <li>Trace the evaluation of expression Stream(1, 2, 3, 4).takeWhile(_ &lt; 3).exists(_ == 3) in the same way it is done for a different expression in Listing 5.3 on page 72 (or shown <a href="http://wisenet.fau.edu/class/scala/notes/Ch05-StrictLazy.html#listing5.3">here</a><a href="http://wisenet.fau.edu/class/scala/notes/Ch05-StrictLazy.html#listing5.3">)</a>, from the textbook. Use the same level of detail.</li>

</ol>

<h1>Problem 2. Unit testing</h1>

Write the solution in a file called p2.scala, in an object called p2. a) Consider the function from Homework 2:   <strong>def</strong> testFunction2[<strong>A</strong>,<strong>B</strong>,<strong>C</strong>](testname<strong>:</strong> <strong>String</strong>, a<strong>:</strong> <strong>A</strong>, b<strong>:</strong> <strong>B</strong>, expected<strong>:</strong> <strong>C</strong>)(f<strong>:</strong> (<strong>A</strong>,<strong>B</strong>) <strong>=&gt;</strong> C)

<strong>:</strong> <strong>Either</strong>[<strong>String</strong>, <strong>String</strong>] <strong>=</strong> {     <strong>val</strong> actual <strong>=</strong> f(a, b)

<strong>if</strong> (actual == expected) <strong>Right</strong>(testname + ” passed”)      <strong>else</strong> <strong>Left</strong>(testname + ” failed”)

}

Write a similar function called <em>testExpression</em> that tests expressions <strong>passed by name </strong>against an expected value and that returns <em>Right</em> or <em>Left</em> in the same way as testFunction2. Here are some examples how to use it:

<strong>def</strong> plus1(x<strong>:</strong> <strong>Int</strong>) <strong>=</strong> x + <strong>1</strong>

<strong>def</strong> plus1_buggy(x<strong>:</strong> <strong>Int</strong>) <strong>=</strong> x – <strong>1</strong>    // buggy by design

println(testExpression(“divide”, <strong>24</strong> / <strong>2</strong>, <strong>12</strong>))  // Right(“divide OK passed”)     println(testExpression(“plus1”, plus1(<strong>1</strong>), <strong>2</strong>))  // Right(“plus1 OK passed”)




println(testExpression(“plus1_buggy”, plus1_buggy(<strong>1</strong>), <strong>2</strong>))      // Left(“plus1_buggy failed”)




<ol>

 <li>Write a polymorphic function called <em>tryOrElse</em> that has two non-strict parameters of type <em>A</em>, called <em>expr</em> and <em>defval</em>. Evaluating expression expr may throw an exception. If no exception is thrown, <em>tryOrElse</em> returns the result from evaluating <em>expr</em>. If <em>expr </em>throws an exception then <em>tryOrElse </em>returns the result from evaluating <em>defval</em>. Make sure <em>expr </em>is evaluated exactly once and <em>defval </em>is forced only in case <em>expr </em>fails with an exception.</li>

</ol>

Here are some examples:

<strong>val</strong> grade <strong>=</strong> tryOrElse(“100”.toInt, <strong>0</strong>)

// grade == 100

<strong>val</strong> gradeBadFormat <strong>=</strong> tryOrElse(“1X00”.toInt, <strong>0</strong>)     // gradeBadFormat == 0

<strong>val</strong> grades <strong>=</strong> <strong>List</strong>[<strong>Int</strong>]()

<strong>val</strong> gpa <strong>=</strong> tryOrElse(grades.sum / grades.length, –<strong>1.0</strong>)     // gpa == -1

Write in p2’s main() function 3 test cases for <em>tryOrElse </em>using the <em>testExpression</em> unit test function from part a) that are different from the examples above.

<ol>

 <li>Use the formal definition to prove that <em>tryOrElse</em> is a non-strict function.</li>

</ol>

<h1>Problem 3. Stream practice</h1>

Write the solution in a file called p3.scala, in an object called p3. Use (import) the version of the Stream types given in the textbook and discussed in class.

<ol start="8">

 <li>Write an expression using the <em>from</em> and <em>find </em>functions to find the first number that divides both 6 and 8.</li>

 <li>Write an expression using the <em>from, filter, take, </em>and <em>map </em>functions that returns a stream with the first 10 square integers greater than a variable <em>n</em> initialized to 1000.</li>

 <li>Use functions <em>from</em>, <em>foldRight</em>, <em>map</em>, and <em>take </em>to write an expression that returns a comma-separated string with the first 10 square numbers: “1,4,9,16,25,36,49,64,81,100,” .</li>

 <li>Consider this stream declaration:</li>

</ol>

<strong>val</strong> st2 <strong>=</strong> <strong>Stream</strong>(<strong>1.1</strong>, <strong>1</strong>, <strong>1.15</strong>, <strong>1.11</strong>, <strong>0.85</strong>, <strong>1.15</strong>, <strong>1.23</strong>)

Write an expression using Stream’s methods and <em>st2</em> that computes the absolute value of the difference between successive elements of <em>st2 </em>and returns <em>Some</em>(<em>d</em>) where <em>d</em> is the first such difference that exceeds 0.2 or returns <em>None</em> if no such difference exceeds that threshold. Do not write a custom recursive function. In our case with <em>st2</em>, the expression should return <em>Some</em>(0.26). Hint: <em>drop </em>and <em>zipWith</em>.

<ol>

 <li>Write a method in object p3 called <em>findIndex</em> that takes as arguments a Stream[A] and, in a separate argument list, a predicate <em>p: A =&gt; Boolean,</em> and that returns the index (starting from 0) in the stream <em>s </em>of the first element <em>a</em> for which <em>p(a)</em> is true. <em>findIndex </em>must use the methods of the Stream trait and should not be a custom recursive function. Example usage:</li>

</ol>

println(findIndex(from(<strong>0</strong>) take <strong>10</strong>)(<strong>_</strong> == <strong>30</strong>))  // None     println(findIndex(from(<strong>0</strong>) take <strong>10</strong>)(<strong>_</strong> &gt;= <strong>5</strong>))   // Some(5)

Write 3 tests using <em>testExpression </em>for testing this function. One should be for the boundary case, when the predicate fails to find any element.

<ol>

 <li>Write a method in object p3 called <em>findPositions</em> that takes as arguments a Stream[A] and, in a separate argument list, a predicate <em>p: A =&gt; Boolean,</em> and that returns a Stream[Int] with the indices (starting from 0) in the stream <em>s </em><strong>for all </strong>elements <em>a</em> for which <em>p(a)</em> is true. If none such values exist, the returned stream must be empty. <em>findPositions</em> must use the methods of the Stream trait and should not be a custom recursive function. Example usage:</li>

</ol>

println(findPositions(<strong>Stream</strong>(<strong>1</strong>, <strong>2</strong>, <strong>1</strong>, <strong>4</strong>, <strong>0</strong>, <strong>1</strong>, <strong>3</strong>) take <strong>20</strong>)(<strong>_</strong> == <strong>1</strong>) toList)        // List(0, 2, 5) println(findPositions(<strong>Stream</strong>(<strong>1</strong>, <strong>2</strong>, <strong>1</strong>, <strong>4</strong>, <strong>0</strong>, <strong>1</strong>, <strong>3</strong>) take <strong>20</strong>)(<strong>_</strong> &gt; <strong>10</strong>) toList) //List()

Write 3 tests using <em>testExpression </em>for testing <em>findPositions</em>. One should be for the boundary case, when the predicate fails to find any element.

<ol>

 <li>Extra credit 3 points * Extra credit 3 points * Extra credit: 3 points</li>

</ol>

Write a method in object p3 called <em>splitWith</em> that takes as arguments a Stream[A] and, in a separate argument list, a predicate <em>p: A =&gt; Boolean,</em> and that returns a tuple  of Stream[A] objects, (<em>sTrue</em>, <em>sFalse</em>) so that stream <em>sTrue </em>gets the elements <em>a</em> for which <em>p(a)</em> is true and stream <em>sFalse </em>gets the elements <em>a</em> for which <em>p(a)</em> is false. s<em>plitWith </em>must use the methods of the Stream trait and should not be a custom recursive function. Example usage:

<strong>val</strong> streams <strong>=</strong> splitWith(<strong>Stream</strong>(<strong>5</strong>, –<strong>3</strong>, <strong>2</strong>, –<strong>1</strong>, <strong>6</strong>, <strong>0</strong>, <strong>3</strong>, <strong>2</strong>))(<strong>_</strong> &lt; <strong>0</strong>)

println(streams._1.toList) // List(-3, -1)

println(streams._2.toList) // List(5, 2, 6, 0, 3, 2)

<h1>Problem 4. Infinite streams and corecursion</h1>

Write the solution in a file called p4.scala, in an object called p4. Use (import) the version of the Stream types given in the textbook and discussed in class.

<ol>

 <li>Write a function called <em>list2stream</em> that takes as parameter a List[A] object reference and returns a Stream[A] with the objects from the list in the given order. Your function <strong>must be corecursive</strong>. Example usage:</li>

</ol>

// converts the list to stream and back to a list:     println(list2stream(<strong>List</strong>(<strong>1</strong>,<strong>2</strong>,<strong>3</strong>,<strong>4</strong>)) toList) // List(1,2,3,4)     println(list2stream(<strong>List</strong>()) toList)        // List()

Write two unit tests in main() with <em>testExpression </em>for this function.

<ol>

 <li>Write a function called <em>list2streamViaUnfold</em> that takes as parameter a List[A] object reference and returns a Stream[A] with the objects from the list in the given order. Your function <strong>must use the unfold function</strong>. (this function does the same thing as <em>list2stream </em>from part a)). Example usage:</li>

</ol>

println(list2streamViaUnfold(<strong>List</strong>(<strong>1</strong>,<strong>2</strong>,<strong>3</strong>,<strong>4</strong>)) toList)  // List(1,2,3,4)     println(list2streamViaUnfold(<strong>List</strong>()) toList)         // List()

Write two unit tests in main() with <em>testExpression </em>for this function.

<ol>

 <li>A linear congruential generator is an algorithm that creates a stream of pseudo-random numbers using the recursive formula:        <em>X</em><em><sub>n</sub></em><sub>+1</sub>=(<em>a X</em><em><sub>n</sub></em>+<em>c</em>)%<em>m </em>.</li>

</ol>

Write a <strong>corecursive </strong>function called <em>rndStream</em> that takes as parameter the initial seed (<em>X</em><em><sub>0</sub></em>) and produces an infinite stream of pseudo-random numbers using this series with values a = 1103515245, m = 2³¹, c = 12345, <strong>and keeping only the 30 least significant bits for the values being returned</strong>. Use the <em>Long</em> data type instead of <em>Int </em>where it is necessary to avoid overflow. Example usage starting with seed = 0:

// List(0, 12345, 1406932606, 654583775, 1449466924,

// 229283573, 1109335178, 1051550459, 1293799192, 794471793) println(rndStream(<strong>0</strong>) take <strong>10</strong> toList)

Check your random numbers against this list. Write a unit test in main() with <em>testExpression </em>for this function.

<ol>

 <li>Write a corecursive function called <em>signs </em>that takes as parameter <em>n: Int </em>that and that returns an infinite stream +n, -n, +n, -n, …. Example:   println(signs(<strong>1</strong>) take <strong>5</strong> toList)  // List(1, -1, 1, -1, 1)</li>

 <li>Extra credit 3 points * Extra credit 3 points * Extra credit: 3 points</li>

</ol>

Write an expression using functions <em>foldRight</em>, <em>signs</em>, <em>take</em>, <em>from</em>, and <em>zipWith </em>that computes the sum

<em>i</em>+1

∑<em><sub>i</sub></em><sub>=1 </sub><sup>(</sup><sup>−1</sup><em><sub>i</sub></em><sup>) </sup>for the first 500 terms. What does the sum of this series represent ?


