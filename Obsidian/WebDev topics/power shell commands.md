1. Delete cache and installed modules
	Remove-Item -Recurse -Force node_modules, .next
	Remove-Item -Force package-lock.json
	npm cache clean --force

2.  Check you node verison 
    node -v
    