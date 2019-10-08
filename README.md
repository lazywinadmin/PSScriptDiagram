# PSScriptDiagram

l idée est de récupe tous les if/where/foreach etc... et à chaque fois qu'on en trouve un, on peut ajouter une description, afin de créer un diagrame !

![plop](classes.png)


```powershell
#Method qui set la description, lorsqu'un format de commentaire special est utilisé
# le format étant le suivant
<#
    DiagramDescription: Blalalalala
#>

#ce commentaire doit être le premier commentaire qui apparait dans le corps du noeud

## a mettre dans la classe node, et faire les différents cas pour les différents type de node
SetDescription () {
        $tokens=@()
        
        Switch ( $this.Type ) {
            "If" { [System.Management.Automation.Language.Parser]::ParseInput($this.raw.Clauses[0].Item2.Extent.Text,[ref]$tokens,[ref]$null) }
        }
        
        $c = $tokens | Where-Object kind -eq "comment"
        If ( $c.count -gt 0 ) {
            If ( $c[0].text -match '\<#\r\s+DiagramDescription:(?<description> .+)\r\s+#\>' ) { $this.Description = $Matches.description.Trim() }
        }
    }
```
