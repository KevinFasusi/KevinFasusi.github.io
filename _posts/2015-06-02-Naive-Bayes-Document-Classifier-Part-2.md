---

layout: post
title: Naive Bayes Document Classifier Part 2
excerpt: Building the beast
---

{% include header.html %}

# Building The Classifier #


Since the objective is to automate the whole process, I wanted to drop the files in the folder and be done with it. Using Microsoft Scripting runtime object library, I used FileSystemObject and textstream instead of the  first sub routine iterates through the files in the directory. The 'classifier training' class has a method for extracting and tokenizing the text a sentence at a time from each file in the folder.

<pre><code class="vbscript" >
Do While filePath <> ""
        Debug.Print filePath
        If filePath <> "ModelData.txt" Then
            Do While fsoReadStream.AtEndOfStream <> True
                sentenceText = fsoReadStream.ReadLine
                sentenceText = RemoveArtifacts(sentenceText)
                If sentenceText <> "" Then
                    sentenceArr() = TokenizeSentence(sentenceText)                
                    ReDim Preserve corpusArr(j + UBound(sentenceArr()))
                        For i = 0 To UBound(sentenceArr())
                            corpusArr(i + j) = sentenceArr(i)
                        Next
                    j = j + UBound(sentenceArr())
                Else: End If       
            Loop
        Else
        End If
        filePath = Dir()
    
        fsoReadStream.Close
        Set fsoSourceFile = fsoSource.GetFile(folderPath & pathSep & filePath)
        Set fsoReadStream = fsoSourceFile.OpenAsTextStream(ForReading, TristateUseDefault)
Loop
</code></pre>

{% highlight Visual Basic.NET linenos %}
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


First thing that wass required was to remove the punctuation from