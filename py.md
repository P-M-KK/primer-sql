# Reference

...

# Cheat Sheet

---

<table>
  <caption>Collections</caption>
  <thead>
    <tr>
      <th>Type</th>
      <th>Syntax</th>
      <th>0</th>
      <th>1</th>
      <th>Example</th>
      <th>Specialized</th>
      <th>ABC</th>
      <th>Properties</th>
      <th>Remarks</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>string</th>
      <td>"" or ''</td>
      <td>""</td>
      <td>"x"</td>
      <td>"foo\\bar"</td>
      <td></td>
      <td>Sequence</td>
      <td>homogenous (char-only), ordered, immutable</td>
      <td>not a collection, but many overlapping operations, so a single string may be interpreted as a char collection accidentally</td>
    </tr>
    <tr>
      <th>bytes</th>
      <td>b"" or b''</td>
      <td>b""</td>
      <td>b"x"</td>
      <td>b"foo\x5Cbar"</td>
      <td>bytearray (mutable)</td>
      <td>Buffer</td>
      <td>homogenous (byte-only), ordered, immutable</td>
      <td>...</td>
    </tr>
    <tr>
      <th>group</th>
      <td>()</td>
      <td></td>
      <td>(x)</td>
      <td>("foo" "\\" "bar")</td>
      <td></td>
      <td></td>
      <td></td>
      <td>not a collection, but included due to syntactic similarity</td>
    </tr>
    <tr>
      <th>tuple</th>
      <td>(,)</td>
      <td>()</td>
      <td>(x,)</td>
      <td>("foo", "\\", "bar")</td>
      <td>namedtuple</td>
      <td>Sequence</td>
      <td>heterogenous, ordered, immutable</td>
      <td></td>
    </tr>
    <tr>
      <th>tuple, implicit</th>
      <td>,</td>
      <td></td>
      <td>x,</td>
      <td>"foo", "\\", "bar"</td>
      <td></td>
      <td></td>
      <td></td>
      <td>avoid for singletons</td>
    </tr>
    <tr>
      <th>list</th>
      <td>[,]</td>
      <td>[]</td>
      <td>[x]</td>
      <td>["foo", "\\", "bar"]</td>
      <td>
        deque (stack/queue)
        <br/>
        array (num/char-only)
        <br/>
        Pandas Series
      </td>
      <td>Sequence</td>
      <td>homogenous, ordered, mutable, dynamic size</td>
      <td></td>
    </tr>
    <tr>
      <th>set</th>
      <td>{,}</td>
      <td>set()</td>
      <td>{x}</td>
      <td>{"foo", "\\", "bar"}</td>
      <td>frozenset (immutable)</td>
      <td>Set</td>
      <td>homogenous, unordered, mutable, dynamic size, unique</td>
      <td></td>
    </tr>
    <tr>
      <th>dict</th>
      <td>{:,}</td>
      <td>{}</td>
      <td>{x:y}</td>
      <td>
        {
          <br/>
          0: "foo",
          <br/>
          1: "\\",
          <br/>
          2: "bar"
          <br/>
        }
      </td>
      <td>
        ChainMap (list of dicts)
        <br/>
        Counter (multiset)
        <br/>
        defaultdict
        <br/>
        OrderedDict (ordered)
        <br/>
        Pandas DataFrame (ordered dict of Pandas Series)
      </td>
      <td>Mapping</td>
      <td>homogenous, unordered, mutable, dynamic size, unique key</td>
      <td></td>
    </tr>
    <tr>
      <th>NumPy NDArray</th>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>homogenous, ordered, mutable, fixed size, n-dimensional</td>
      <td></td>
    </tr>
  </tbody>
</table>

<table>
  <caption>Comprehensions</caption>
  <thead>
    <tr>
      <th>Type</th>
      <th>Syntax</th>
      <th>Example</th>
      <th>Remarks</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>list</th>
      <td>[]</td>
      <td>[x for x in my_iterable]</td>
      <td></td>
    </tr>
    <tr>
      <th>set</th>
      <td>{}</td>
      <td>{x for x in my_iterable}</td>
      <td></td>
    </tr>
    <tr>
      <th>dict</th>
      <td>{:}</td>
      <td>{k: v for k, v in my_iterable}</td>
      <td></td>
    </tr>
    <tr>
      <th>generator</th>
      <td>()</td>
      <td>(x for x in my_iterable)</td>
      <td>lazy</td>
    </tr>
  </tbody>
</table>

---

<table>
  <caption>Strings</caption>
  <thead>
    <tr>
      <th>Type</th>
      <th>Syntax</th>
      <th>Example</th>
      <th>Remarks</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>normal</th>
      <td>"" or ''</td>
      <td>"foo\\bar"</td>
      <td></td>
    </tr>
    <tr>
      <th>raw</th>
      <td>r""</td>
      <td>r"foo\bar"</td>
      <td></td>
    </tr>
    <tr>
      <th>formatted</th>
      <td>f""</td>
      <td>
        var = "bar"
        <br/>
        f"foo\\{var}"
      </td>
      <td></td>
    </tr>
    <tr>
      <th>byte</th>
      <td>b""</td>
      <td>b"foo\x5Cbar"</td>
      <td></td>
    </tr>
    <tr>
      <th>block</th>
      <td>
        """
        <br/>
        """
      </td>
      <td>
        """foo\\
        <br/>
        bar"""
      </td>
      <td></td>
    </tr>
    <tr>
      <th>block, group</th>
      <td>
        (""
        <br/>
        "")
      </td>
      <td>
        ("foo\\"
        <br/>
        "bar")
      </td>
      <td>adjacent string literals within an expression are concatenated automatically</td>
    </tr>
    <tr>
      <th>block, operator</th>
      <td>
        "" \
        <br/>
        ""
      </td>
      <td>
        "foo\\" \
        <br/>
        "bar"
      </td>
      <td>
        avoid
        <br/>
        adjacent string literals within an expression are concatenated automatically
      </td>
    </tr>
  </tbody>
</table>

<table>
  <caption>String Interpolation</caption>
  <thead>
    <tr>
      <th>Type</th>
      <th>Syntax</th>
      <th>Example</th>
      <th>Remarks</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>inline</th>
      <td>f"{x}"</td>
      <td>f"foo\\{var}"</td>
      <td>evaluation upon definition</td>
    </tr>
    <tr>
      <th>function</th>
      <td>"{}".format(x)</td>
      <td>"foo\\{}".format(var)</td>
      <td></td>
    </tr>
    <tr>
      <th>function, indexed</th>
      <td>"{#}".format(x)</td>
      <td>"foo\\{0}".format(var)</td>
      <td></td>
    </tr>
    <tr>
      <th>function, named</th>
      <td>"{y}".format(y=x)</td>
      <td>"foo\\{v}".format(v=var)</td>
      <td></td>
    </tr>
    <tr>
      <th>operator</th>
      <td>"%s" % x</td>
      <td>"foo\\%s" % var</td>
      <td>deprecated</td>
    </tr>
  </tbody>
</table>

---

<table>
  <caption>Collection Unpacking</caption>
  <thead>
    <tr>
      <th>Type</th>
      <th>Syntax</th>
      <th>Example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>literal</th>
      <td>*x</td>
      <td>*my_collection &rarr; x, y,</td>
    </tr>
    <tr>
      <th>literal, dict</th>
      <td>**x</td>
      <td>**my_dict &rarr; x: a, y: b,</td>
    </tr>
    <tr>
      <th>assignment</th>
      <td>x, y</td>
      <td>x, y = my_2_collection</td>
    </tr>
    <tr>
      <th>assignment, partial</th>
      <td>x, *y, z</td>
      <td>first, *rest, last = my_collection</td>
    </tr>
  </tbody>
</table>

<table>
  <caption>Variadic Functions</caption>
  <thead>
    <tr>
      <th>Type</th>
      <th>Syntax</th>
      <th>Example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>packed tuple of positional</th>
      <td>*x</td>
      <td>
        def my_func(*args): pass
        <br/>
        my_func(1, 2, 3)
      </td>
    </tr>
    <tr>
      <th>packed dict of keyword</th>
      <td>**x</td>
      <td>
        def my_func(**kwargs): pass
        <br/>
        my_func(x=1, y=2, z=3)
      </td>
    </tr>
    <tr>
      <th>explicit positional, either, & keyword</th>
      <td>x, /, y, *, z</td>
      <td>
        def my_func(x, /, y, *, z): pass
        <br/>
        my_func(1, 2, z=3)
        <br/>
        my_func(1, y=2, z=3)
      </td>
    </tr>
  </tbody>
</table>

---

<table>
  <caption>Line Manipulation</caption>
  <thead>
    <tr>
      <th>Type</th>
      <th>Syntax</th>
      <th>Example</th>
      <th>Remarks</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1 string, many lines</th>
      <td>
        """
        <br/>
        """
      </td>
      <td>
        """foo\\
        <br/>
        bar"""
      </td>
      <td>for readability</td>
    </tr>
    <tr>
      <th>1 collection, many lines</th>
      <td>(,) or [,] or {,} or {:,}</td>
      <td>
        (x
        <br/>
        , y)
      </td>
      <td>for readability</td>
    </tr>
    <tr>
      <th>1 expression, many lines</th>
      <td>()</td>
      <td>
        (x
        <br/>
        + y)
      </td>
      <td>for readability</td>
    </tr>
    <tr>
      <th>1 statement, many lines</th>
      <td>\</td>
      <td>
        x \
        <br/>
        + y
      </td>
      <td>avoid</td>
    </tr>
    <tr>
      <th>many similar statements, 1 line</th>
      <td>, ,</td>
      <td>x, y = 1, 2</td>
      <td>avoid</td>
    </tr>
    <tr>
      <th>many statements, 1 line</th>
      <td>;</td>
      <td>x = 1; y++</td>
      <td>for REPL</td>
    </tr>
    <tr>
      <th>1 clause, 1 line</th>
      <td>:</td>
      <td>if x > y: x++</td>
      <td>for 1-liners</td>
    </tr>
  </tbody>
</table>

---

# Resources

...
