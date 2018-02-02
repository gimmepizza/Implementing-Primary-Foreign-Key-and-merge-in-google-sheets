## Implementing-Primary-Foreign-Key-and-merge-in-google-sheets
Google sheets is a very good platform for implementing basic databases for events especially when using google forms in parallel.
For having dynamic merging of two forms with a primary key or implementing foreign key is necessary.
For merging there are google sheet ad-ons , but hardly do they work , the ones that work ,work for only 100 columns ,
for larger requirements use the code described below: 

# Foreign key implementation:
Quite easy ...

```
=IMPORTRANGE("url of file from which data is coming","Sheet name from which data is coming!A2:f1000")
```
After this the cell would show error ( quite normal) just hover above it  and a popup would open on its side, click on **Allow access**
and you are good to go.

This is just the basic usage of importing stuff from other sheets.

I used the above to provide view access to just a few columns of my master sheet.

you can dig deeper and attach conditions on the data too , [IMPORTRANGE](https://support.google.com/docs/answer/3093340?hl=en)

# Merging with primary key(for more than 100 rows)
I found no direct solution for merging 600-800 rows of data from two google sheet responses onto a sheet using a unique token id ,

so this a very absurd but working way to get the job done.

Since the forms are dynamic , both the sheets do not have all the token ids  ( i.e one form may be ahead in token number in comparison to the other)

So we begin by sorting the form1 responses ( **Assumption:** form one is ahead of form2 in the number of token ids it has.)

Use [SORT](https://support.google.com/docs/answer/3093150?hl=en) to get the relevant data sorted according to the primary key column(in our case the token id column)
```
=sort(Form_responses_1!A2:P1360,Form_responses_1!B2:B1360,True)
```
First input is the range to be sorted , the second input is the column according to which the sorting is to be done. True tells the sheet to sort it in ascending order.

Next we need to add the data from the form2 responses in front of the respective column you ,according to the token id ,

For this we use the [MATCH](https://support.google.com/docs/answer/3093378?hl=en) function ,to match the the token id from one column to the other , but the problem with match is that it returns a relative position of matching w.r.t  the given range.

In order to get around this we need the help of another function called, [INDEX](https://support.google.com/docs/answer/3098242?hl=en), this function outputs the content of the cell , when the input is relative x and y position of cell w.r.t. to the range.
```
=index(Form_responses_2!A2:E1000,match(F2,Form_responses_2!A2:A1000),1)
```
First input is the range , second input is relative x position , third input is relative y position.

Done...

Now just copy this formula for the rest of content of form2 responses keeping in mind to match using the primary key column only , and just changing the relative x postion for different data.
now this is done, drag down the cells to accomodate the formula for all the cells you need.

**When no data is filled , the columns show N/A, I didn't find thatt to be a problem , since I had to handle the MASTER SHEET, , this is not an error**

# BONUS: FILTER ,Get relevant data from a sheet filled with all type of data
Basic aim : Get all the data from a row according to the empptynessof a certain column.
```
=filter(MASTER!A2:E1000,not(isblank(MASTER!G2:G1000)))
```
First input is the range for filtering,

Second input is a conditional statement, can have if else conditions too.

[FILTER](https://support.google.com/docs/answer/3093197?hl=en)

[ISBLANK](https://support.google.com/docs/answer/3093290?hl=en)

[IF](https://support.google.com/docs/answer/3093364?hl=en)

Some other functions that may help :

[VLOOKUP](https://support.google.com/docs/answer/3093318?hl=en)

[HLOOKUP](https://support.google.com/docs/answer/3093375?hl=en)
