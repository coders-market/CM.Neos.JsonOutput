# Getting Started

This is a package to show one way to implement an additional json view for Neos pages. The JSON view get rendered 
by adding a `.json` to the url (instead of `.html`);

## Quickstart:
1) `composer install`
2) Run neos: `./flow server:run`
3) Access Site on `http://127.0.0.1:8081`
4) Run setup and create/import a page

**Attention!**  in your project roots `Configuration/Routes.yaml` has to be the definition to your subroutes.

```
#project root: Configuration/Routes.yaml
-
  name: 'Neos Json Output'
  uriPattern: '<CmSubRoutes>'
  subRoutes:
    'CmSubRoutes':
      package: 'CM.Neos.JsonOutput'
```

If you want to see the output in JSON change the format .html to .json

The root page can not be changed to json view so far, as you can't add .json to the url
If you need a page with a complete tree, you could create a page and add a menu to the content


Some (not all!) default Neos NodeTypes are added already (see `Resources/Private/Fusion/Root.fusion`) others have to be 
added e.g. to your site package. Every NodeType which is used and should show up as a valid json representation has 
to be declared.


## Custom NodeType as valid json

A teaser NodeType which has the properties: title, image and text 

_Root.fusion:_
```
prototype(Vendor.PackageName:TeaserJson) < prototype(CM.Neos.JsonOuput:JsonObject) {

	title = ${q(node).property('title')}

	imageUri = Neos.Neos:ImageUri {
		asset = ${q(node).property('image')}
		width = 100
		height = 100
		allowCropping = TRUE
		allowUpScaling = TRUE
	}

	text = ${q(node).property('text')}

}
```

and added to the `ContentCase` for JSON also in `Root.fusion

```
# Add every type you use to this Content Case list to get rendered as json
prototype(CM.Neos.JsonOuput:ContentCaseJson) < prototype(Neos.Fusion:Case) {

    [...]

	teaser {
		condition = ${q(node).is('[instanceof CM.Neos.JsonOuput:Teaser]')}
		type = ${'CM.Neos.JsonOuput:TeaserJson'}
	}

	headline {
		condition = ${q(node).is('[instanceof Neos.NodeTypes:Headline]')}
		type = ${'CM.Neos.JsonOuput:HeadlineJson'}
	}

	[...]

}
```

# Credits

This approach is inspired by and based on Dimitri Pisarev solution
