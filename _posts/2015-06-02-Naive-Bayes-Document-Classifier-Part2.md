---

layout: post
title: Naive Bayes Document CLassifier
excerpt: Just
---

{% include header.html %}

## Building The Classifier ##

Since the objective is to automate the whole process, I wanted to drop the filed in the folder and be done with it. Using Microsoft Scripting runtime object library, I used FileSystemObject and textstream instead of the  first sub routine iterates through the files in the directory.


{% highlight vba %}
Private Function TokenizeSentence(sentence As String) As Variant
Dim i, counter As Integer
Dim bagOfCharArr() As String
Dim bagOfWordsArr() As String

counter = 0
    bagOfCharArr() = Split(sentence)
    ReDim bagOfWordsArr(UBound(bagOfCharArr()))
        For i = 0 To UBound(bagOfCharArr())
            If Not Replace(Trim(bagOfCharArr(i)), " ", "") = "" Then
                bagOfWordsArr(counter) = bagOfCharArr(i)
                counter = counter + 1
            Else: End If
        Next
    If counter <> 0 Then
    ReDim Preserve bagOfWordsArr(counter - 1)
    Else: End If
        
wordArr() = bagOfWordsArr()
TokenizeSentence = bagOfWordsArr()
End Function
{% endhighlight %}
