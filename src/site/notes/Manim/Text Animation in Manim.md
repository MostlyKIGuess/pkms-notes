Hey there traveler, Welcome to the ship 🍻!

Manim has something called mobjects, and text is one of them, you will read this word frequently so , it is just a set of objects that are available for you to write manim code.

- Manim has two types of Text Rendering, one is Simple Text which uses [Pango Lib](https://pango.gnome.org/) which can be useful to render non-English alphabets.And can be called by using:
  ```python
  text = Text("Hello world", font_size=144)
  # or 
    text = MarkupText(
		f'all in red <span fgcolor="{YELLOW}">except 
	    this</span>', color=RED
        )
```
- While the other is called Tex( which you will majorly use) which is used like this:
```python
tex = Tex(r"\LaTeX", font_size=144)
```
![[Pasted image 20240109224418.png]]
```python
#it allows attribute changing, like color for eg:
tex = Tex(r'Hello \LaTeX', color=BLUE, font_size=144)
```
![[Pasted image 20240109224500.png]]
```python
#you can write math function like:
 tex = MathTex(r'f(x) &= 3 + 2 + 1\\ &= 5 + 1 \\ &= 6', font_size=96)
```
![[Pasted image 20240109224801.png]]

