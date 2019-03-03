# css-grid-layout
Posted 02072019, Manuel Matuzo's "The Dark Side of the Grid" describes the CSS Grid Layout with its potential implementation pitfalls.

'CSS Grid Layout - The first true layout method in CSS. A grid-based layout system designed for two dimensional layouts. Properties like float, displey: inline-block, position, and display: table were not originally intended for building layouts.  
Flexbox - distributes available space and places items either vertically or horizontally. FLexbox loses a lot of its flexibility as soon as you're applying flex-wrap and add widths to your flex items. Therefore, you probably need grid.' 

According to Rachel Andrew-'Modern CSS Layout, power and responsibility', the following example illustrates the issue of using 'Grid to flatten out document structure in order that all elements become a child of the element with the Grid declared--Making layout simple...'

Example: (html)
<section>
    <h2>Pink Floyd discography</h2>
    <ul>
        <li>The Piper at the Gates of Dawn</li>
        <liA Saucerful of Secrets</li>
        <li>More</li>
        <li>Ummagumma</li>
        <li>Atm Heart Mother</li>
        <li>Meddle</li>
        <li>Obscured by Clouds</li>
        <li>The Dark Side of the Moon</li>
    </ul>
</section>
'The section forms a 3-column grid, but we want the heading to span all the columns, and each li should fill one cell.'

Therefore, we select the section keyword, set display: grid, add 3 even columns, a 10px gutter and we amke the heading span all 3 columns.

Example: (CSS)
section {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-gap: 10px;
}

h2 {
    grid-column: 1 / -1;
}

What happens here doesn't look like what was expected. 'Only direct children of the grid container will align with the grid. In this example, those are the h2 and the ul, but we want all the li elements to fill cells in the grid.'

'Solution #1: Flattening the document structure
If the placement algorithm only affects direct child elements, we'll just make our li direct children by removing the ul and swapping the li's for div's to avoid invalid html. It's a solution be not a good one because we're compromising on semantics for design reasons.'

'Flattening the document structure may have bad effects on the semantics of your document, which is especially bad for screen readers users. For example, when you're using a list, screen readers usually announce the number of list items which helps with navigation an overview. Also, flat document might be harder to read when displayd without CSS.'

'Why would someone disable CSS? It's unlikely that users disable CSS on purpose but sometimes an error occurs or the connection is just so slow that only the html displays successfully.' It's best not to flatten document structures.

'Solution #2: Creating a subgrid
The arguably best solutions would be to use subgrids. A grid item can itself be a grid container with its own column and row definitions, It can also be a grid container but defer the definitions of rows and columns to its parent grid container.'

Example:
    ul {
        display: grid;
        grid-template-columns: subgrid;
    }

'By setting the value of grid-template-columns to subgrid on the unordered list, the list items now align with the parent grid.' Unfortunately, support for subgrid is so bad, it doesn't even have a caniuse page. Subgrids are not supported in any browser yet.'

'Solution #3: Using display: contents
An alternative to using subgrids is a different property that has a similar effect. If you set the display value of an element to contents, it will act as if it got replaced by its child items.'

Example:
    ul {
        display: contents;
    }

'This causes the list items to take part in the alignment of the sections grid because for them the parent ul doesn't exist anymore. However, Edge doesn't support it.'