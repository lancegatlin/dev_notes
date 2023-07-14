How to make command+arrow go to begining and end of line in iterm2?
https://superuser.com/questions/585518/how-to-make-commandarrow-go-to-begining-and-end-of-line-in-iterm2
Go to settings (⌘ Command+,)
Go to tab Keys
Under "Key Bindings"
Change entry ⌘ Command← to Send Hex code: 0x01
Change entry ⌘ Command→ to Send Hex code: 0x05
///
You can also set up Option+arrow keys to jump words by setting up Option + left arrow to escape sequence and key b, and Option + right arrow to escape sequence and key f. – 
Jamon Holmgren
 Sep 22, 2013 at 23:33
@JamonHolmgren - I can't see escape sequence in the menu, the menu is huge! which sub-section is it in? – 
Toni Leigh
 Oct 19, 2015 at 11:39
@ToniLeigh Just scan down until you start seeing "Send..." items. It's in there. – 
Jamon Holmgren
 Nov 12, 2015 at 0:16
Here's a link to a screenshot: cloud.githubusercontent.com/assets/1479215/11107105/… – 
Jamon Holmgren
 Nov 12, 2015 at 0:18
I think this should be included as a default keybinding. – 
Prince Odame
 Dec 4, 2020 at 7:53
///
Note: need to remove profile key bindings for option-left/right in Settings -> Profile -> Keys 
