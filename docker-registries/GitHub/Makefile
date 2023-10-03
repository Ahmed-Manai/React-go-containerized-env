
# have to autenticate to ghcr.io first
# https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry#authenticating-to-the-container-registry
# then use this link: https://github.com/settings/tokens/new?scopes=write:packages
# ghcr.io/ahmed-manai = <fixed>/<NameSpace or UserName >
.PHONY: push-github-packages
push-github-packages:
	docker tag my-scratch-image ghcr.io/ahmed-manai/my-scratch-image #default latest
	docker push ghcr.io/ahmed-manai/my-scratch-image

	docker tag my-scratch-image ghcr.io/ahmed-manai/my-scratch-image:abc-123 #specific tag
	docker push ghcr.io/ahmed-manai/my-scratch-image:abc-123