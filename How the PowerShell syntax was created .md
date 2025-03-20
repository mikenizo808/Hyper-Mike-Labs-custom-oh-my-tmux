# How the PowerShell syntax was created for my oh my tmux

## Examples

You can run these examples to practice or refine the techniques used.
At the end of the file you can see the syntax that was used to create
the "number of people in space" entry for the "oh my tmux" config.

## weather

The original `curl` syntax for weather in "oh my tmux"

    ## Example curl syntax
    ## curl -f -s -m 2 wttr.in/Chicago?format=3

## PowerShell weather (with `Invoke-WebRequest`)

    Invoke-WebRequest -Uri "wttr.in/Chicago?u&format=3" | Select-Object -ExpandProperty Content

## PowerShell weather (with "Invoke-RestMethod")

    $error.Clear();Clear-Host
    $uri = "wttr.in/Chicago?u&format=4"
    $objRest = Invoke-RestMethod -Uri $uri -Verbose
    $objRest

## Number of people in space

The original pyhton example. Cheers and credit to the original authors. We later convert this to PowerShell.

    ## python example from
    ## https://github.com/raspberrypilearning/people-in-space-indicator/blob/master/worksheet.md

    #original
    import requests

    url = "http://api.open-notify.org/astros.json"

    r = requests.get(url)
    j = r.json()
    n = j['number']
    print(n)
    

## Converted to PowerShell Invoke-RestMethod

You could use `Invoke-WebRequest`, but we will use the better `Invoke-RestMethod` for this case.

    $uri = "http://api.open-notify.org/astros.json"
    $objRest = Invoke-RestMethod -Uri $uri -Verbose
    $objRest

    $info = [PSCustomObject]@{
        NumPeopleInSpace = $objRest.Number
        People           = $objRest.People
    }

## Show output overall

    return $info

## show details of people in space

    $info.People

#### example output for $info.People

    craft    name
    -----    ----
    ISS      Oleg Kononenko
    ISS      Nikolai Chub
    ISS      Tracy Caldwell Dyson
    ISS      Matthew Dominick
    ISS      Michael Barratt
    ISS      Jeanette Epps
    ISS      Alexander Grebenkin
    ISS      Butch Wilmore
    ISS      Sunita Williams
    Tiangong Li Guangsu
    Tiangong Li Cong
    Tiangong Ye Guangfu



## Crafting text report

    $error.Clear();Clear-Host
    Write-Host ('There are {0} people in space' -f $info.NumPeopleInSpace) -ForegroundColor Green

    $objCrewInfo = $info.People
    $objSpaceShips = $info.People.craft | Select-Object -Unique
    $numberOfSpaceShips = $objSpaceShips | Measure-Object | Select-Object -ExpandProperty Count
    $strSpaceShipNames = $objSpaceShips -join ', '
    $strSpaceShipNames

    Write-Host ('There are {0} people in space across {1} space ships' -f $info.NumPeopleInSpace, $numberOfSpaceShips)  -ForegroundColor Green

    $error.Clear();Clear-Host
    Write-Host ('There are {0} people in space across {1} space ships ( {2} )' -f $info.NumPeopleInSpace, $numberOfSpaceShips, $strSpaceShipNames) -ForegroundColor Green


## Show a crew report

    $crewReport = @()
    foreach($crewMember in $objCrewInfo){
        $crewReport += [PSCustomObject]@{
            Craft       = $crewMember.craft
            CrewMember  = $crewMember.name
        }
    }
    $crewReport

    ## Syntax "There are x people on the y"
    $error.Clear();Clear-Host
    $group = $crewReport | Group-Object -Property Craft
    foreach($item in $group){
        Write-Host ('There are {0} people on the {1}' -f $item.Count, $item.Name) -ForegroundColor Green
        Start-Sleep -Seconds 2
    }

    #Or, "Aboard the y there are x crew members"
    $group = $crewReport | Group-Object -Property Craft
    foreach($item in $group){
        Write-Host ('Aboard the {0}, there are {1} crew members' -f $item.Name, $item.Count) -ForegroundColor Green
        Start-Sleep -Seconds 2
    }

## call powershell using pwsh

    $error.Clear();Clear-Host
    $strCommand = '(Invoke-RestMethod -Uri http://api.open-notify.org/astros.json).Number'
    $bytes = [System.Text.Encoding]::Unicode.GetBytes($strCommand)
    $encodedCommand = [Convert]::ToBase64String($bytes)
    pwsh -encodedcommand $encodedCommand -NoProfile -OutputFormat Text #-WindowStyle Hidden

## Output to UTF16 text file

There is no need to save to disk, but you can.

    $encodedCommand | Out-File -Encoding UTF-16 -Path ~/.local/get-powershell-weather-asBase64.txt

    test-path ~/.local/get-powershell-weather-asBase64.txt
    cat ~/.local/get-powershell-weather-asBase64.txt

## Welcome to Base Cakes

Where you can get any recipe to cook space cakes, or check the number of people in space.
For now it just supports number of people in space!

    KABJAG4AdgBvAGsAZQAtAFIAZQBzAHQATQBlAHQAaABvAGQAIAAtAFUAcgBpACAAaAB0AHQAcAA6AC8ALwBhAHAAaQAuAG8AcABlAG4ALQBuAG8AdABpAGYAeQAuAG8AcgBnAC8AYQBzAHQAcgBvAHMALgBqAHMAbwBuACkALgBOAHUAbQBiAGUAcgA=

## full command

    pwsh -noprofile -encodedcommand KABJAG4AdgBvAGsAZQAtAFIAZQBzAHQATQBlAHQAaABvAGQAIAAtAFUAcgBpACAAaAB0AHQAcAA6AC8ALwBhAHAAaQAuAG8AcABlAG4ALQBuAG8AdABpAGYAeQAuAG8AcgBnAC8AYQBzAHQAcgBvAHMALgBqAHMAbwBuACkALgBOAHUAbQBiAGUAcgA=

## contents of "tmux.conf.local"

This goes inside the "tmux_conf_theme_status_right" variable

    #(pwsh -noprofile -encodedcommand KABJAG4AdgBvAGsAZQAtAFIAZQBzAHQATQBlAHQAaABvAGQAIAAtAFUAcgBpACAAaAB0AHQAcAA6AC8ALwBhAHAAaQAuAG8AcABlAG4ALQBuAG8AdABpAGYAeQAuAG8AcgBnAC8AYQBzAHQAcgBvAHMALgBqAHMAbwBuACkALgBOAHUAbQBiAGUAcgA=; sleep 60) ,

## with spacers around new item

This opens with a `|` and closes with a "," so it can be pasted between existing items.

    | people in space #(pwsh -noprofile -encodedcommand KABJAG4AdgBvAGsAZQAtAFIAZQBzAHQATQBlAHQAaABvAGQAIAAtAFUAcgBpACAAaAB0AHQAcAA6AC8ALwBhAHAAaQAuAG8AcABlAG4ALQBuAG8AdABpAGYAeQAuAG8AcgBnAC8AYQBzAHQAcgBvAHMALgBqAHMAbwBuACkALgBOAHUAbQBiAGUAcgA=; sleep 60) ,

> Note: See the current release of "hyper mike labs custom oh my tmux configuration" text file for the latest syntax for your tmux bars (i.e. status right, etc.). See [https://github.com/mikenizo808/Hyper-Mike-Labs-custom-oh-my-tmux](https://github.com/mikenizo808/Hyper-Mike-Labs-custom-oh-my-tmux) and check the releases on the right side.

