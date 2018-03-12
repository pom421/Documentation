## React

### Cycle de vie

- `shouldComponentUpdate` : retourne un booléen pour savoir si le render doit s'exécuter ou non. C'est donc le dev qui peut décider si le rendu doit se faire ou non

- `composant fonctionnel` : composant sans state (principalement pour les composants uniquement UI)


 ### HOC 
 
 Réutiliser un composant simple, fonctionnel et l'encapsuler dans un autre composant qui lui fait une chose plus spécifique.
 
 Ici, une liste générique qui sait comment s'afficher et une liste qui récupère des informations à partir d'une requête Ajax.

```js
import React from "react"
import ListGeneric from "./ListGeneric"

export default class ListUsers extends React.Component {

    constructor(props) {
        super(props)
        this.state = { data: null }
        this.getAjaxData()
    }

    getAjaxData() {
        fetch("http://jsonplaceholder.typicode.com/users").then(response => {
            return response.json()
        }).then(json => {
            const resJson = json.map(elt => { return { id: elt.id, data: elt.username } })
            this.setState({ data: resJson })
        }).catch(err => {
            console.error("Erreur lors du fetch", err)
        })
    }

    render() {

        return (
            <ListGeneric data={this.state.data} />
        )
    }

}
```

```
import React from "react"

export default class ListGeneric extends React.Component {

    constructor(props) {
        super(props)
    }

    render() {
        if (this.props.data && this.props.data instanceof Array) {
            const mkLis = this.props.data.map(elt => <li key={elt.id}>{elt.data}</li>)
            return (
                <ul>
                    {mkLis}
                </ul>
            )
        }
        return ""

    }
}
```
