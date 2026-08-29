# ExtendScripts

# Automation & Scripting: Find Bit Field Discrepancies (FrameMaker ExtendScript)

## Context

At Avago Technologies (Broadcom), I documented read channel chip specifications in FrameMaker. Bit field names like `GLOBALTH1[3:0]` had to stay internally consistent across a document. When the same bit field appeared with two different bracketed ranges, for example `GLOBALTH1[3:0]` in one place and `GLOBALTH1[6:0]` in another, it signaled a real inconsistency that needed manual review and correction.

Checking for this by hand across a long document was slow and error-prone. In 2012, I wrote a FrameMaker ExtendScript to automate the search. It was my first major ExtendScript project, and I documented it heavily as I went, partly to teach myself, partly so I could pick it back up months later without relearning it from scratch.

## What it does

The script searches the active FrameMaker document for bit field patterns, builds a list of every occurrence and its bracketed range, then flags any bit field name that appears with more than one range. It outputs the discrepancies to a new document, which the user then references while manually correcting the source file. It doesn't auto-correct anything. FrameMaker's document object model doesn't give you a safe way to do a blind find-and-replace on something this pattern-dependent, so the script's job is to point a human at the right place, not to make the edit itself.

## Readme

```
PURPOSE
This script searches the active document to find discrepancies in bit field ranges.
For example, BLAH[2:0] and BLAH[3:0] are inconsistent ranges. The script creates a
new blank document listing all bit fields with discrepant ranges. You can use the
list as a reference while you manually search the source document to correct the
discrepancies. This script was designed for use with read channel documentation
standards, where bit fields must always be in uppercase letters and include a
valid range.

ASSUMPTIONS AND CONSIDERATIONS
- As with any ExtendScript, you should save your document before you begin. You
  can also make a backup copy of your document.
- ExtendScripts work only with FrameMaker 10, not earlier versions.
- You must save the script and the file named BlankTemplate together in the same
  folder.
- Bit field names are all uppercase, begin with a letter, and end in either a
  letter or a number. The script uses the following search parameter:
  [A-Z]*[0123456789A-Z]. Search options are: Consider Case, Use Wildcards,
  Whole Words.
- The script works on one document at a time, not books.
- If you use the WriterComment conditional tag, you can hide it before running
  the script (unless you want WriterComment bit fields included in the list).

KNOWN ISSUES
- The script excludes SVG files in its search results. Discrepancies in SVG
  files are missed.
- The script does not handle curly brackets. Bit fields with curly brackets
  such as BLAH{1..2}[2:0] are evaluated as if there are no brackets whatsoever.
- The script considers a space between the bit field name and range as a
  discrepancy. BLAH [2:0] is not treated the same as BLAH[2:0].
- The script will not include more than a seven-character range. A mistyped
  range like [32:132] gets truncated to [32:132 in the discrepancy list.
```

## The code

The header comment documents not just what the script does, but how it got there. I kept the change history because it shows the actual design progression, from a naive copy-paste approach through three rewrites to the final version.

```javascript
//This script. . .
//-->Searches the currently opened document for bit field names.
//-->Creates a new document with a simple list of all discrepant bit fields.
//   Discrepant bit fields are those where the bracketed ranges differ from one
//   occurrence to the next. For example BLAH[3:0], BLAH[4:0], BLAH.
//-->Considers both bit fields with and without brackets.
//-->Does not cycle through a book.
//-->Does not evaluate bit field names and ranges against a separate list. It
//   only checks the active document for internal consistency.
//-->Does not search SVG graphics.
//-->Does not evaluate curly brackets {}.
//-->Does not tell where the bit field discrepancies appear in the document.
//   Only lists the bit field names and ranges. The user must search the
//   document by hand to find them.
//-->Does not change the conditional text settings. The user needs to turn off
//   WriterComment if that's important.

//Written by Jason Langkamer-Smith, February 2012. This was my first major
//ExtendScripts project. I documented the snot out of it so I wouldn't forget
//what I did to make it work. I hope this also helps to teach others.

//Change history. . .
//This first implementation created a separate document and a list of bit
//fields by simply copying and pasting the terms into the new document.
//Learned how to create a new document.
//The second implementation added the list of bit fields to a table, which
//could more easily be sorted to look for inconsistencies. But it was still a
//by-hand process. Learned how to create tables.
//The third implementation did away with the copy and paste approach and
//instead built an array of bit field objects that included a bit field name
//and a bit field range. It sorted the array, pruned the array to show only
//inconsistent bit fields, and then added the resultant array to the output
//document. This cut the list down from 14 pages to 1, showing only two bit
//field discrepancies for the Read QMON chapter.
//This final implementation improved the code by moving portions of the logic
//into separate functions, deleting old logic from previous implementations,
//and adding extensive documentation.
```

One function shows the kind of edge-case reasoning the script had to handle. FrameMaker's API doesn't give you regex over an arbitrary range, so extending a search one character at a time, with a hard stop, was the workaround for finding the closing bracket of a bit field range:

```javascript
function findBracketForBitField (FoundText){
//This function is called if the findBracket function did find a left bracket.
//It gets the entire bracket range.
//This function starts from the end of the FoundText text range, and then
//extends the text range until one of the following conditions is met:
    //-->A closing bracket ] is found.
    //-->Constants.FTI_String is false.
    //-->The range is extended past 7 characters.
//We can use 7 as a hard stop for the range because a bracketed range will not
//usually exceed 7 characters. For example [15:11].
//This 7-character limitation however may introduce some inconsistencies in
//the data if the bracketed range was mistyped. For example
//BLAH[15. . . 13:9. . .7]. In this example, the function will return the
//following 7 characters: [15. . 
//The function also handles the condition where the bit field is in a table
//cell and extending the range beyond 7 results in an invalid parameter
//because the range exceeds the length of the cell.
//The function also handles the case where the right bracket is missing,
//since it will not extend past 7 characters.
//The function returns the string FoundBracketText, representing the bit
//field range.

    var thisDoc = app.ActiveDoc;
    var tr = new TextRange();
    var FoundBracketText = new TextRange();
    var textString = "";
    var textItems = new TextItems();

    tr.beg.obj = FoundText.beg.obj;
    tr.end.obj = FoundText.end.obj;
    tr.beg.offset = tr.end.offset = FoundText.end.offset;

    for (var i = 0; (i < 7)&&(textString != "]") ; i++)
    {
        tr.end.offset = tr.end.offset + 1;
        textItems = thisDoc.GetTextForRange (tr, Constants.FTI_String);

        if (textItems.length > 0)
        {
            textString = (CreateStringFromTextItems(textItems));
            tr.beg.offset = tr.beg.offset + 1;
            }
        else
        {
            break
            }
    }

    tr.beg.offset = FoundText.end.offset;
    tr.end.offset = tr.beg.offset + i;
    textItems = thisDoc.GetTextForRange (tr, Constants.FTI_String);
    textString = (CreateStringFromTextItems(textItems));

    return (textString);
}
```

## Why this matters

I'm not presenting this as a software engineering sample. It's a technical writer solving a documentation-quality problem with code, in an environment (FrameMaker's object model, ExtendScript) that has no modern tooling or community support to lean on. The parts worth noticing are the same instincts I bring to writing docs: documenting known limitations up front instead of letting a user discover them, explaining *why* a design decision was made (the 7-character cutoff) rather than just what it does, and keeping a visible record of earlier approaches and why they were replaced.