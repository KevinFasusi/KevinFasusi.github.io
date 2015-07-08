---

layout: post
title: Assembling Adafruit LCD
description: a little solder here, a little prayer there a dash of code...aaand I broke it!
---
{% include header.html %}

For a project, I recently 

{% highlight ruby %}
def show
  @widget = Widget(params[:id])
  respond_to do |format|
    format.html # show.html.erb
    format.json { render json: @widget }
  end
end
{% endhighlight %}