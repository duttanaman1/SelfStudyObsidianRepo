---
tags:
  - linker-exclude
---
Loyalty Offers
[[Kafka]], [[Redis]] Caching

Completable Future

Axon Kafka Async messaging

Bulk upload Translations

Dev cloud Instances

Performance Improvement for translations offers

Quickly & Automatically Reflect Live Offer Edit and User Roles

Offer Clone enhancement
Bulk Add/Edit offer locales with Status
Multi locale support for Translations Bulk Upload

Adding Redemption Criteria of TRID
	(Persona Based Offers)
Immediate Patch Re;eases
	(Bulk Submit Validations)
Checkmarx Vulnerabilities

UI modernization

Microfrontend
	Used Webpack's Module Federation for runtime Integration
	 Mixing Angular and React in same Application
	 Proper dependency sharing configuration for both frameworks
	 Each Microfrontend can be developed and deployed independently
	 Microfrontend are loaded dynamically at runtime

Main Host Webpack.config.js
```
plugin:[
	new ModuleFedrrationPlugin({
		name: "host",
		remotes:{
			React: "React@http://localhost:3001/react-remoteEntry.js",
			shellApp: "http://localhost:4200/remoteentry.js"
		},
		exposes{},
		shared: {
			...deps, 
		}
	})
]
```

React Host Webpack.config.js
```
plugin:[
	new ModuleFedrrationPlugin({
		name: "host",
		remotes:{},
		exposes{
			"./offerLifecycleWrapper",
			"./src/feature/classification/component/ClassificationWrapper"
		},
		shared: {
			...deps, 
		}
	})
]
```

Angular Host Webpack.config.js
```
plugin:[
	new ModuleFedrrationPlugin({
		library: {type: Module}
		name: "host",
		remotes:{},
		exposes{
			"./offerlifecycle.module.ts",
			"./src/feature/classification/component/classification.module.ts"
		},
		shared: {
			...deps, 
		}
	})
]
```



This Phoenix uses composition at runTime instead of buildTime

Forage Migration Tester
