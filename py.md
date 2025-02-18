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
      <th>Specialized</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>group</th>
      <td>()</td>
      <td></td>
      <td>(x)</td>
      <td></td>
    </tr>
    <tr>
      <th>tuple</th>
      <td>(,)</td>
      <td>()</td>
      <td>(x,)</td>
      <td>namedtuple</td>
    </tr>
    <tr>
      <th>tuple, alt</th>
      <td>,</td>
      <td></td>
      <td>x,</td>
      <td></td>
    </tr>
    <tr>
      <th>list</th>
      <td>[,]</td>
      <td>[]</td>
      <td>[x]</td>
      <td>dequeue</td>
    </tr>
    <tr>
      <th>set</th>
      <td>{,}</td>
      <td>set()</td>
      <td>{x}</td>
      <td>frozenset</td>
    </tr>
    <tr>
      <th>dict</th>
      <td>{:,}</td>
      <td>{}</td>
      <td>{x:y}</td>
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>list</th>
      <td>[]</td>
    </tr>
    <tr>
      <th>set</th>
      <td>{}</td>
    </tr>
    <tr>
      <th>dict</th>
      <td>{:}</td>
    </tr>
    <tr>
      <th>generator</th>
      <td>()</td>
    </tr>
  </tbody>
</table>

<table>
  <caption>Strings</caption>
  <thead>
    <tr>
      <th>Type</th>
      <th>Syntax</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>normal</th>
      <td>""</td>
    </tr>
    <tr>
      <th>raw</th>
      <td>r""</td>
    </tr>
    <tr>
      <th>formatted</th>
      <td>f""</td>
    </tr>
    <tr>
      <th>byte</th>
      <td>b""</td>
    </tr>
    <tr>
      <th>block</th>
      <td>""""""</td>
    </tr>
    <tr>
      <th>block, alt</th>
      <td>("" "")</td>
    </tr>
    <tr>
      <th>block, alt (avoid)</th>
      <td>"" \ ""</td>
    </tr>
  </tbody>
</table>

<table>
  <caption>String Interpolation</caption>
  <thead>
    <tr>
      <th>Type</th>
      <th>Syntax</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>inline</th>
      <td>f"{x}"</td>
    </tr>
    <tr>
      <th>postfix</th>
      <td>"{}".format(x)</td>
    </tr>
    <tr>
      <th>printf</th>
      <td>"%s" % x</td>
    </tr>
  </tbody>
</table>

<table>
  <caption>Collection Unpacking</caption>
  <thead>
    <tr>
      <th>Type</th>
      <th>Syntax</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>string, tuple, list, set</th>
      <td>*x</td>
    </tr>
    <tr>
      <th>dict</th>
      <td>**x</td>
    </tr>
    <tr>
      <th>sequence</th>
      <td>x, y</td>
    </tr>
    <tr>
      <th>iterable</th>
      <td>x, *y, z</td>
    </tr>
  </tbody>
</table>

<table>
  <caption>Function Definition Variadic Arguments</caption>
  <thead>
    <tr>
      <th>Type</th>
      <th>Syntax</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>packed tuple</th>
      <td>*x</td>
    </tr>
    <tr>
      <th>packed dict</th>
      <td>**x</td>
    </tr>
    <tr>
      <th>positional, either, keyword</th>
      <td>x, /, y, *, z</td>
    </tr>
  </tbody>
</table>

# Resources

...
