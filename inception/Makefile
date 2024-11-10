name = inception

data_dir = ~/data
wp_dir = $(data_dir)/wordpress_files
db_dir = $(data_dir)/database

all:
	@printf "Creating necessary directories for ${name}...\n"
	@mkdir -p $(wp_dir) $(db_dir)
	@printf "Starting ${name} services...\n"
	@docker-compose -f ./srcs/docker-compose.yml up -d

build:
	@printf "Creating necessary directories for ${name}...\n"
	@mkdir -p $(wp_dir) $(db_dir)
	@printf "Building ${name} infrastructure...\n"
	@docker-compose -f ./srcs/docker-compose.yml up -d --build

down:
	@printf "Shutting down ${name} environment...\n"
	@docker-compose -f ./srcs/docker-compose.yml down

re:
	@printf "Recreating ${name} setup from scratch...\n"
	@mkdir -p $(wp_dir) $(db_dir)
	@docker-compose -f ./srcs/docker-compose.yml up -d --build

clean: down
	@printf "Tidying up ${name} resources and removing data directories...\n"
	@docker system prune -a --force
	@docker volume rm srcs_wordpress_files srcs_database 2>/dev/null || true
	@rm -rf $(wp_dir) $(db_dir)

fclean:
	@printf "Performing a full purge of ${name} components...\n"
	@if [ -n "$$(docker ps -qa)" ]; then docker stop $$(docker ps -qa) && docker rm $$(docker ps -qa); else echo "No active containers to terminate."; fi
	@docker system prune --all --force --volumes
	@docker network prune --force
	@docker volume rm srcs_wordpress_files srcs_database 2>/dev/null || true
	@printf "Removing data directories for a clean state...\n"
	@rm -rf $(wp_dir) $(db_dir)

.PHONY: all build down re clean fclean
