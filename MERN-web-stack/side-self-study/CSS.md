# Cascading Style Sheets (CSS)

## Definition of Cascading Style Sheets (CSS)

CSS, or Cascading Style Sheets, is a language used to describe the presentation of a document written in HTML or XML. While HTML structures the content, CSS handles the visual and aural layout, making web pages look attractive and user-friendly.

## What is CSS Used For?
1. **Styling Web Pages**: CSS is primarily used to control the look and feel of web pages, including layout, colors, and fonts.

2. **Responsive Design**: CSS enables responsive design, which ensures web pages look good on all devices (desktops, tablets, and phones).

3. **Separation of Concerns**: By separating content (HTML) from design (CSS), it's easier to maintain and update web pages.

## Basic Syntax
CSS rules are composed of selectors and declarations. 

Here's a simple example:
```
selector {
  property: value;
}
```
- **Selector**: Targets the HTML element you want to style (e.g., p for paragraphs, .class-name for class selectors).

- **Property**: The aspect of the element you want to change (e.g., color, font-size).

- **Value**: Specifies the setting for the property (e.g., red, 16px).

## Common Properties
1. **Color**: Sets the color of text.
```
p {
  color: blue;
}
```
2. **Font**: Specifies font-related properties.
```
h1 {
  font-family: Arial, sans-serif;
  font-size: 24px;
}
```
3. **Background**: Defines background properties.
```
body {
  background-color: #f0f0f0;
}
```
4. **Margin and Padding**: Controls space around and inside elements.
```
div {
  margin: 20px;
  padding: 10px;
}
```
5. **Borders**: Sets border properties.
```
img {
  border: 2px solid black;
}
```
6. **Layout**: Utilizes properties like *display*, *flex*, and *grid* for complex layouts.
```
.container {
  display: flex;
  justify-content: space-between;
}
```

## Conclusion
Understanding CSS is crucial for creating visually appealing, responsive, and well-structured web pages. With practice, you can master various CSS properties and techniques to enhance the user experience.