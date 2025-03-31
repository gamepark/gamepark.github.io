# Upgrade the framework

To upgrade the Game Park framework in a game, you need to:
- upgrade `@gamepark/rules-api` and `@gamepark/react-game` in `app/package.json`
- upgrade `@gamepark/rules-api` in `rules/package.json`
- (optional) if you use new features from the rules-api (like new utility functions), upgrade `@gamepark/rules-api` in the peerDependencies of `rules/package.json`
