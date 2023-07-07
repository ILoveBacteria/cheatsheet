# HTML Cheatsheet
HTML is a markup language that is used to create web pages. HTML is not a programming language, but it is often used with CSS and JavaScript to create interactive web pages.

HTML is easy to learn - You will enjoy it!

## Table Of Contents

- [HTML Cheatsheet](#html-cheatsheet)
  - [Table Of Contents](#table-of-contents)
  - [Basic Elements](#basic-elements)
  - [Text](#text)
  - [List](#list)
  - [Tables](#tables)
  - [Media](#media)
  - [Semantic HTML](#semantic-html)
  - [Forms](#forms)
    - [Form Elements](#form-elements)
    - [Form Attributes](#form-attributes)
    - [Form Validators](#form-validators)


## Basic Elements

| Tag            | Description                                                     |
| -------------- | --------------------------------------------------------------- |
| `<html>`       | Defines the root of an HTML document                            |
| `<head>`       | Defines the metadata of an HTML document                        |
| `<title>`      | Defines the title of an HTML document                           |
| `<body>`       | Defines the body of an HTML document                            |
| `<br>`         | Inserts a single line break                                     |
| `<!-- ... -->` | Defines a comment                                               |
| `<div>`        | Defines a section in a document                                 |
| `<a>`          | Anchor element is used to create hyperlinks in an HTML document |


## Text

| Tag              | Description                                                                             |
| ---------------- | --------------------------------------------------------------------------------------- |
| `<p>`            | Defines a paragraph                                                                     |
| `<h1>` to `<h6>` | Defines HTML headings                                                                   |
| `<em>`           | Defines an italic text                                                                  |
| `<strong>`       | Defines a bold text                                                                     |
| `<span>`         | Defines an inline container for text and can be used to group text for styling purposes |

## List

| Tag    | Description               |
| ------ | ------------------------- |
| `<ul>` | Defines an unordered list |
| `<ol>` | Defines an ordered list   |
| `<li>` | Defines a list item       |

## Tables

| Tag       | Description                          |
| --------- | ------------------------------------ |
| `<thead>` | Groups the header content in a table |
| `<tbody>` | Groups the body content in a table   |
| `<tfoot>` | Groups the footer content in a table |
| `<tr>`    | Defines a row in a table             |
| `<th>`    | Defines a header cell in a table     |
| `<td>`    | Defines a cell in a table            |

## Media

| Tag       | Description              |
| --------- | ------------------------ |
| `<img>`   | Defines an image         |
| `<video>` | Defines a video or movie |
| `<audio>` | Defines sound content    |

## Semantic HTML

| Tag            | Description                                                                                 |
| -------------- | ------------------------------------------------------------------------------------------- |
| `<header>`     | Defines a header for a document or section                                                  |
| `<footer>`     | Defines a footer for a document or section                                                  |
| `<nav>`        | Defines a set of navigation links                                                           |
| `<article>`    | Defines an independent self-contained article                                               |
| `<section>`    | Defines a section in a document                                                             |
| `<aside>`      | Defines content aside from the page content                                                 |
| `<main>`       | Specifies the main content of a document                                                    |
| `<figcaption>` | Specifies a caption for a `<figure>` element                                                |
| `<figure>`     | Specifies self-contained content, like illustrations, diagrams, photos, code listings, etc. |

## Forms

### Form Elements

| Tag          | Description                                                |
| ------------ | ---------------------------------------------------------- |
| `<form>`     | Defines an HTML form for user input                        |
| `<input>`    | Defines an input control                                   |
| `<textarea>` | Defines a multiline input control (text area)              |
| `<button>`   | Defines a clickable button                                 |
| `<select>`   | Defines a drop-down list                                   |
| `<datalist>` | Specifies a list of pre-defined options for input controls |
| `<option>`   | Defines an option in a drop-down list                      |
| `<label>`    | Defines a label for an `<input>` element                   |
| `<fieldset>` | Groups related elements in a form                          |
| `<legend>`   | Defines a caption for a `<fieldset>` element               |

### Form Attributes

| Attribute   | Description                                                                |
| ----------- | -------------------------------------------------------------------------- |
| action      | Specifies where to send the form-data when a form is submitted             |
| method      | Specifies the HTTP method to use when sending form-data                    |
| name        | Specifies the name of a form                                               |
| for         | Specifies which form element a label is bound to                           |
| step        | Specifies the legal number intervals for an input field                    |
| placeholder | Specifies a short hint that describes the expected value of an input field |
| value       | Specifies the value of an input field                                      |
| readonly    | The input field can not be modified                                        |
| encType     | The form encode type                                                       |

### Form Validators

| Attribute | Description                                                                   |
| --------- | ----------------------------------------------------------------------------- |
| required  | Specifies that an input field must be filled out before submitting the form   |
| max       | Specifies the maximum value for an input field                                |
| min       | Specifies the minimum value for an input field                                |
| pattern   | Specifies a regular expression that an input field's value is checked against |
| minlength | Specifies the minimum number of characters allowed in an input field          |
| maxlength | Specifies the maximum number of characters allowed in an input field          |
| accept    | Specifies the types of files that the server accepts (only for type="file")   |
