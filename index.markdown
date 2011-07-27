---
layout: site
---

# Forms with Alternate Submits

Imagine a shopping cart with fields to update the quantities. At the bottom
you'll probably see an "Update Totals" button and a "Buy!" button.

<form method="post">
<table>
<tr>
<th>quantity</th>
<th>description</th>
<th>price</th>
<th>total</th>
</tr>
<tr>
<td><input type="number" name="qty_1" id="qty_1" value="2"/></td>
<td><label for="qty_1">fine widget</label></td>
<td style="text-align: right;">$2.00</td>
<td style="text-align: right;">$4.00</td>
</tr>
<tr>
<td><input type="number" name="qty_2" id="qty_2" value="1"/></td>
<td><label for="qty_2">medium-grade widget</label></td>
<td style="text-align: right;">$1.50</td>
<td style="text-align: right;">$1.50</td>
</tr>
<tr>
<th colspan="4" style="text-align: right;">grand total: $5.50</th>
</tr>
</table>
<div style="text-align: right;">
<button type="submit" name="update">Update Totals</button>
<button type="submit" name="buy">Buy!</button>
</div>
</form>

When you hit the "Update Totals" button, you expect to come back to the same
page you're currently on, but with all the dollar amounts updated. When you
hit "Buy", you expect the server to pay attention to the quantities that you
changed, but continue on with the purchase process.

But how do you handle this on the server? How do you know which button the
user pressed? Well, that depends.

## Form Fields

When a form gets submitted, the names and values of each `<textarea>` and
`<select>` element and most `<input>` elements get sent to the server either
in the URL or the `POST` body.

`<input>`s of the type "radio" or "checkbox" only get sent to the server if
they are checked. An unchecked checkbox or radio button doesn't send some kind
of "false" value to the server -- it's just not there.

## Buttons

There are two ways to make buttons in HTML. The most obvious is to use the
`<button>` element, with a type of "button" to create a generic button or a
type of "submit" to create a button that will submit the form. You can also
give it a type of "reset" to create a button that sets all the fields back to
the state they had when the page loaded.

    <button type="button">a generic button</button>
    <button type="submit">a submit button</button>
    <button type="reset">a reset button</button>

The other way is with the `<input>` button. If you give it a type of "button"
you get a generic button, and a type of "submit" makes a button that submits
the form.

    <input type="button" value="a generic button">
    <input type="submit" value="a submit button">
    <input type="reset" value="a reset button">

`<input>`s of the type "button" or "reset" don't get sent to the server.
`<input>`s of the type "submit" get sent to the server if they are "clicked".

In most browsers, `<button>`s of type "button" or "reset" are just like
`<input>`s of the same type -- they don't get sent to the server.  Likewise, a
`<button>` of type "submit" usually only gets sent to the server when it's
clicked.

I say "usually" because in IE 7 and older, all `<button>`s of type "button"
get sent to the server no matter what. Even worse, IE 6 on back will send
"submit" and "reset" `<button>`s as well - even ones that you didn't click.

There is one more quirk around `<button>`s in old versions of IE. IE 7 and
older send the contents of the `<button>` instead of its value. The upshot of
this is that you should just ignore the values of buttons and only pay
attention to their names.

## Submitting on Enter

There is one more way to submit a form -- hitting the `enter`/`return` key
while inside a text input.

IE exhibits the simpler behavior here, just sending the form values on to the
server as if you had clicked a nameless submit button.

All other browsers will pick the first "submit" `<input>` or `<button>` and
act like you clicked it.

Basically, the default action of the form has to be the same action as
triggered by the first "submit" in order to have the same functionality across
browsers. You can solve this by always having your default "submit" be the
first in source order. However, [the default button should always be on the
right][default right], at least in languages written left-to-right.

Unless you want to employ some CSS trickery to display buttons out-of-order,
your best bet is to add an invisible, nameless, valueless "submit" at the top
of the form that will trigger the default action.

To hide this normalizing "submit", you can't just give it `display: hidden;`,
either. Otherwise, Webkit-based browsers will ignore it completely. I like
giving it a `visibility: hidden;` and putting it inside a div with `height: 0;
width: 0; overflow: hidden;`.

## What Does All This Mean?

As far as I can tell, your non-default submit button can't be a `<button>`
element - it has to be an `<input>` of type "submit". This is a real shame
because `<input>`s are much less styleable than `<button>`s.

You will also want to make a non-visible, nameless, valueless `<input>` of
type "submit" at the beginning of your form to ensure that your default action
is triggered when the user hits `enter` inside a text field.

[default right]: http://www.fastcodesign.com/1664046/design-basics-flow-is-why-ok-buttons-are-always-on-the-right
