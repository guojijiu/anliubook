### run from repository root

### makefile里不要乱用TAB，只有命令所在的行才能且只能以TAB开头！

### Example:

```
make pro
make test
make local
```

```
PROJECT_ROOT = `pwd`
GIT = `which git`
COMPOSER = `which composer`
PHP = `which php`

.PHONY: pro
local:
	cd $(PROJECT_ROOT)
	$(GIT) pull
	$(COMPOSER) dump
	$(PHP) artisan config:clear
	$(PHP) artisan route:clear
	$(PHP) artisan clear-compiled
	echo update success.
test:
	cd $(PROJECT_ROOT)
	$(GIT) pull
	$(COMPOSER) clearcache
	$(PHP) -d memory_limit=-1 $(COMPOSER) install -vvv
	$(PHP) artisan config:cache
	$(PHP) artisan route:cache
	$(PHP) artisan optimize
	$(PHP) artisan migrate --force
	echo update success.
pro:
	cd $(PROJECT_ROOT)
	$(GIT) pull
	$(COMPOSER) clearcache
	$(PHP) -d memory_limit=-1 $(COMPOSER) install --no-dev -vvv
	$(PHP) artisan config:cache
	$(PHP) artisan route:cache
	$(PHP) artisan optimize
	$(PHP) artisan migrate --force
	echo update success.
```
