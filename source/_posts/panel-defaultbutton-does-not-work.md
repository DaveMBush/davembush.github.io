---
title: Panel DefaultButton does not work
tags:
  - asp.net
  - Button
  - DefaultButton
  - ImageButton
  - LinkButton
  - Panel
url: 397.html
id: 397
categories:
  - ASP.NET
date: 2012-12-25 13:16:36
---

![misc_vol2_051](/uploads/2008/09/misc-vol2-051.jpg) Or, at least, it didn't. Here's what happened: Yesterday a client asked me to help track down a problem he was having with setting the default button for a text box. As you should already know, you can make ASP.NET automatically click a specific button on the page (Button, ImageButton, or LinkButton) by grouping everything in a Panel and setting the DefaultButton property to the ID of the button you want to have automatically clicked.  Of course, in situations like these, you can pull your hair out trying to find the problem. There was nothing obviously wrong with the code. Everything was grouped into a panel. All of the Panels had their DefaultButton properties set to the correct button. Maybe it's because it is an ImageButton? No, not only are ImageButtons supported, but creating a quick demo verified that they did in fact work. But what's this? When I put the ASCX file into design view, it does not render the panel with more than a 1px high rectangle. That's odd... wonder why it's doing that. Let's try resizing it. Nope. That doesn't work. And then I looked closer. All of the code basically looked something like this:  

<tr>
  <asp:Panel DefaultButton="m_ibKeyIngredient" ID="m_pKeyIngredient" runat="server" Width="370px">
    <td valign="top">
      <asp:TextBox ID="m_tbKeyIngredient" runat="server" Width="244px"></asp:TextBox>
      <asp:ImageButton ID="m_ibKeyIngredient" runat="server" OnClick="m\_ibKeyIngredient\_Click" ImageUrl="images/gobutton.gif" ImageAlign="AbsBottom" />
    </td>
  </asp:Panel>
</tr> 

If you casually read over this code, you completely miss the fact that the TD elements are INSIDE of the Panel. You see the TR element and the panel and if you aren't thinking clearly, you think the TR element is the cell when in fact, it is the row. The TD elements that represent the cell are further in and easy to miss. You need to keep in mind that the Panel renders itself as a DIV and you can't have a DIV tag inside a TR tag. It turns out that swapping the Panel tag with the TD element solved the whole problem. Unfortunately, this took us several hours of frustration to figure out. So, what did we learn?

1.  If your ASP.NET client side code isn't working as you'd expect, validate your HTML.
2.  When working on a problem, if you see another problem that is part of the same code, it is not always inappropriate to take a detour to try to fix that problem. It may in fact fix the problem you are working on. If nothing else, it will give your mind a break.
