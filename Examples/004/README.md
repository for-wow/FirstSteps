# First Steps: Example 4 - Adding functionality to existing functions

**I don't speak English. It's google translate. Sorry.**

---

There is a hook method to add new functionality to an existing function. This method is that I create a local variable and assign it the function that I want to change. And I create a new function whose name is the same as the function that I want to change.

For example, I want to display the value of an ItemString in a tooltip when hovering over an item in a bag. The GameTooltip.SetBagItem function is responsible for this type of tooltip.

1. I keep the original function:
```
local HookSetBagItem = GameTooltip.SetBagItem
```

2. Then I create a new function by repeating the name and parameters of the original function. Since the SetBagItem function belongs to the GameTooltip object, I need to add the self reference to its input parameters:
```
local HookSetBagItem = GameTooltip.SetBagItem
function GameTooltip.SetBagItem(self, container, slot)

end
```

3. I do not need to completely change the functionality of this function, I need to add a new one to the existing functionality. So inside the new function, I call the original function to display the tooltip:
```
local HookSetBagItem = GameTooltip.SetBagItem
function GameTooltip.SetBagItem(self, container, slot)
    HookSetBagItem(self, container, slot)
end
```
I run the code, check it and see the standard tooltip:

![Image-1](img/1.png)

4. Since I repeated the parameters of the original function, in the new function I can get the value of the "bag number" and "cell number in the bag" parameters and use these parameters to get the ItemLink value:
```
local HookSetBagItem = GameTooltip.SetBagItem
function GameTooltip.SetBagItem(self, container, slot)
	HookSetBagItem(self, container, slot)
	local ItemLink = GetContainerItemLink(container, slot)
end
```

5. Empty bag cells will return an empty ItemLink, so I need to check for availability. And from ItemLink I get ItemString using regular expression:
```
local HookSetBagItem = GameTooltip.SetBagItem
function GameTooltip.SetBagItem(self, container, slot)
	HookSetBagItem(self, container, slot)
	local ItemLink = GetContainerItemLink(container, slot)
	if ItemLink then
		local _, _, ItemString = strfind(ItemLink, "|H(.+)%[")
	end
end
```

6. Now I add a line to the tooltip:
```
local HookSetBagItem = GameTooltip.SetBagItem
function GameTooltip.SetBagItem(self, container, slot)
HookSetBagItem(self, container, slot)
	local ItemLink = GetContainerItemLink(container, slot)
	if ItemLink then
		local _, _, ItemString = strfind(ItemLink, "|H(.+)%[")
		GameTooltip:AddLine("|n" .. ItemString, 1, 1, 1)
	end
end
```
I run the code, check it and see that my string has been added to the standard tooltip. But the dimensions of the tooltip have remained standard and my string is outside the tooltip:

![Image-2](img/2.png)

7. To fix this, I can calculate how many pixels to increase the height of the tooltip and set a new height for it, or I can call the GameTooltip:Show() function, which automatically sets the tooltip to the height according to its content  and shows the tooltip:
```
local HookSetBagItem = GameTooltip.SetBagItem
function GameTooltip.SetBagItem(self, container, slot)
	HookSetBagItem(self, container, slot)
	local ItemLink = GetContainerItemLink(container, slot)
	if ItemLink then
		local _, _, ItemString = strfind(ItemLink, "|H(.+)%[")
		GameTooltip:AddLine("|n" .. ItemString, 1, 1, 1)
		GameTooltip:Show()
	end
end
```

I run the code, check it and see the modified tooltip:

![Image-3](img/3.png)
