.PHONY: compile-circuit
compile-circuit: ## Compile the circuit
	@zokrates compile -i main.zok

.PHONY: generate-witness
generate-witness: ## Generate a witness for the circuit
	@zokrates compute-witness -a 5 781

.PHONY: setup
setup: ## Run Groth16 trusted setup for the circuit
	@zokrates setup

.PHONY: generate-proof
generate-proof: ## Generate a proof for the circuit
	@zokrates generate-proof

.PHONY: verify
verify: ## Verify the proof
	@zokrates verify

.PHONY: clean
clean: ## Clean the generated files
	@rm -rf abi.json out out.r1cs witness verification.key proving.key proof.json out.wtns

.PHONY: help
help: ## Displays this help
	@awk 'BEGIN {FS = ":.*##"; printf "Usage:\n  make \033[1;36m<target>\033[0m\n\nTargets:\n"} /^[a-zA-Z0-9_-]+:.*?##/ { printf "  \033[1;36m%-25s\033[0m %s\n", $$1, $$2 }' $(MAKEFILE_LIST)