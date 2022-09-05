# Example 2 - Change PVP status icon

**I don't speak English. It's google translate. Sorry.**

---

1. You must rewrite the standard function `PlayerFrame_UpdatePvPStatus()`

2. You must set the path to your image. This image should be in the folder (or under the folder) of your addon "ADDON_NAME". (Name "pvp.tga" just an example.)
```
PlayerPVPIcon:SetTexture("Interface\\AddOns\\ADDON_NAME\\pvp")
```

3. Most likely, for the new icon, you will need to specify different sizes and a different offset (so that it looks neat on the screen). To do this, you must erase the previous size and position data.
```
PlayerPVPIcon:ClearAllPoints()
```

4. Now you need to set a new position. Choose the X and Y offset values as you like. You can bind the icon not to the left if you want.
```
PlayerPVPIcon:SetPoint("LEFT", PlayerPVPIcon:GetParent(), 20, 10)
```

5. Set the height and width (the same as the height and width of your image).
```
PlayerPVPIcon:SetWidth(32)
PlayerPVPIcon:SetHeight(32)
```

As a result, it should look like this:
```
function PlayerFrame_UpdatePvPStatus()
    local factionGroup, factionName = UnitFactionGroup("player");
 
    PlayerPVPIcon:SetTexture("Interface\\AddOns\\ADDON_NAME\\pvp")
    PlayerPVPIcon:ClearAllPoints()
    PlayerPVPIcon:SetPoint("LEFT", PlayerPVPIcon:GetParent(), 20, 10)
    PlayerPVPIcon:SetWidth(32)
    PlayerPVPIcon:SetHeight(32)
    PlayerPVPIcon:Show()
 
    if ( UnitIsPVPFreeForAll("player") ) then
        -- Setup newbie tooltip
        PlayerPVPIconHitArea.tooltipTitle = PVPFFA;
        PlayerPVPIconHitArea.tooltipText = NEWBIE_TOOLTIP_PVPFFA;
        PlayerPVPIconHitArea:Show();
    elseif ( factionGroup and UnitIsPVP("player") ) then
        -- Setup newbie tooltip
        PlayerPVPIconHitArea.tooltipTitle = factionName;
        PlayerPVPIconHitArea.tooltipText = getglobal("NEWBIE_TOOLTIP_"..strupper(factionGroup));
        PlayerPVPIconHitArea:Show();
    else
        PlayerPVPIcon:Hide();
        PlayerPVPIconHitArea:Hide();
    end
end
```

---

If you want to display different icons for Alliance and Horde:<br>
(`ADDON_NAME` need to change to the name of your addon, in this example the icon names are `Icon-Alliance` and `Icon-Horde`)

```
function PlayerFrame_UpdatePvPStatus()
    local factionGroup, factionName = UnitFactionGroup("player");
    if ( UnitIsPVPFreeForAll("player") ) then
        PlaySound("igPVPUpdate");
        PlayerPVPIcon:SetTexture("Interface\\TargetingFrame\\UI-PVP-FFA");
        PlayerPVPIcon:Show();
 
        -- Setup newbie tooltip
        PlayerPVPIconHitArea.tooltipTitle = PVPFFA;
        PlayerPVPIconHitArea.tooltipText = NEWBIE_TOOLTIP_PVPFFA;
        PlayerPVPIconHitArea:Show();
    elseif ( factionGroup and UnitIsPVP("player") ) then
        PlaySound("igPVPUpdate");
        PlayerPVPIcon:SetTexture("Interface\\AddOns\\ADDON_NAME\\Icon-"..factionGroup);
        PlayerPVPIcon:ClearAllPoints() -- The icons I have chosen are too big and I need to set their size and position
        PlayerPVPIcon:SetPoint("LEFT", PlayerPVPIcon:GetParent(), 15, 10)
        PlayerPVPIcon:SetWidth(32)
        PlayerPVPIcon:SetHeight(32)
        PlayerPVPIcon:Show();
 
        -- Setup newbie tooltip
        PlayerPVPIconHitArea.tooltipTitle = factionName;
        PlayerPVPIconHitArea.tooltipText = getglobal("NEWBIE_TOOLTIP_"..strupper(factionGroup));
        PlayerPVPIconHitArea:Show();
    else
        PlayerPVPIcon:Hide();
        PlayerPVPIconHitArea:Hide();
    end
end
```
