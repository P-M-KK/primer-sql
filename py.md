# Reference

...

# Cheat Sheet

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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>group</th>
      <td>()</td>
      <td></td>
      <td>(x)</td>
      <td>("foo" "\\" "bar")</td>
      <td></td>
    </tr>
    <tr>
      <th>tuple</th>
      <td>(,)</td>
      <td>()</td>
      <td>(x,)</td>
      <td>("foo", "\\", "bar")</td>
      <td>namedtuple</td>
    </tr>
    <tr>
      <th>tuple, implicit</th>
      <td>,</td>
      <td></td>
      <td>x,</td>
      <td>"foo", "\\", "bar"</td>
      <td></td>
    </tr>
    <tr>
      <th>list</th>
      <td>[,]</td>
      <td>[]</td>
      <td>[x]</td>
      <td>["foo", "\\", "bar"]</td>
      <td>dequeue</td>
    </tr>
    <tr>
      <th>set</th>
      <td>{,}</td>
      <td>set()</td>
      <td>{x}</td>
      <td>{"foo", "\\", "bar"}</td>
      <td>frozenset</td>
    </tr>
    <tr>
      <th>dict</th>
      <td>{:,}</td>
      <td>{}</td>
      <td>{x:y}</td>
      <td>{<br/>0: "foo",<br/>1: "\\",<br/>2: "bar"<br/>}</td>
      <td>
        ChainMap
        <br/>
        Counter
        <br/>
        OrderedDict
        <br/>
        defaultdict
      </td>
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>list</th>
      <td>[]</td>
      <td>[x for x in my_iterable]</td>
    </tr>
    <tr>
      <th>set</th>
      <td>{}</td>
      <td>{x for x in my_iterable}</td>
    </tr>
    <tr>
      <th>dict</th>
      <td>{:}</td>
      <td>{k: v for k, v in my_iterable}</td>
    </tr>
    <tr>
      <th>generator</th>
      <td>()</td>
      <td>(x for x in my_iterable)</td>
    </tr>
  </tbody>
</table>

<table>
  <caption>Strings</caption>
  <thead>
    <tr>
      <th>Type</th>
      <th>Syntax</th>
      <th>Example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>normal</th>
      <td>"" or ''</td>
      <td>"foo\\bar"</td>
    </tr>
    <tr>
      <th>raw</th>
      <td>r""</td>
      <td>r"foo\bar"</td>
    </tr>
    <tr>
      <th>formatted</th>
      <td>f""</td>
      <td>var = "bar"<br/>f"foo\\{var}"</td>
    </tr>
    <tr>
      <th>byte</th>
      <td>b""</td>
      <td>b"foo\x5Cbar"</td>
    </tr>
    <tr>
      <th>block</th>
      <td>"""<br/>"""</td>
      <td>"""foo\\<br/>bar"""</td>
    </tr>
    <tr>
      <th>block, group</th>
      <td>(""<br/>"")</td>
      <td>("foo\\"<br/>"bar")</td>
    </tr>
    <tr>
      <th>block, operator (avoid)</th>
      <td>"" \<br/>""</td>
      <td>"foo\\" \<br/>"bar"</td>
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>inline</th>
      <td>f"{x}"</td>
      <td>f"foo\\{var}"</td>
    </tr>
    <tr>
      <th>function</th>
      <td>"{}".format(x)</td>
      <td>"foo\\{}".format(var)</td>
    </tr>
    <tr>
      <th>function, indexed</th>
      <td>"{#}".format(x)</td>
      <td>"foo\\{0}".format(var)</td>
    </tr>
    <tr>
      <th>function, named</th>
      <td>"{y}".format(y=x)</td>
      <td>"foo\\{v}".format(v=var)</td>
    </tr>
    <tr>
      <th>operator (deprecated)</th>
      <td>"%s" % x</td>
      <td>"foo\\%s" % var</td>
    </tr>
  </tbody>
</table>

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
      <th>string, tuple, list, set</th>
      <td>*x</td>
      <td></td>
    </tr>
    <tr>
      <th>dict</th>
      <td>**x</td>
      <td></td>
    </tr>
    <tr>
      <th>left</th>
      <td>x, y</td>
      <td>x, y = my_2_sequence</td>
    </tr>
    <tr>
      <th>left, partial</th>
      <td>x, *y, z</td>
      <td>first, *rest, last = my_sequence</td>
    </tr>
  </tbody>
</table>

<table>
  <caption>Function Definition Variadic Arguments</caption>
  <thead>
    <tr>
      <th>Type</th>
      <th>Syntax</th>
      <th>Example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>packed tuple</th>
      <td>*x</td>
      <td></td>
    </tr>
    <tr>
      <th>packed dict</th>
      <td>**x</td>
      <td></td>
    </tr>
    <tr>
      <th>positional, either, keyword</th>
      <td>x, /, y, *, z</td>
      <td></td>
    </tr>
  </tbody>
</table>

## Misc

adjacent strings within an expression are concatenated automatically

semicolon combines multiple lines of the same scope/indent

# Resources

...
