run-dev:
	KAFKA_BROKER_ADDRESS='localhost:19092' \
	source setup_credentials.sh && \
	source setup_live_config.sh && \
	poetry run python src/main.py

run-dev-historical:
	KAFKA_BROKER_ADDRESS='localhost:19092' \
	source setup_credentials.sh && \
	source setup_historical_config.sh && \
	poetry run python src/main.py

build:
	docker build -t kafka-to-feature-store .

run: build
	docker run \
		--network=redpanda_network \
		--env KAFKA_BROKER_ADDRESS=redpanda-0:9092 \
		--env KAFKA_TOPIC=ohlc \
		--env KAFKA_CONSUMER_GROUP=ohlc_consumer_group_99 \
		--env FEATURE_GROUP_NAME=ohlc_feature_group \
		--env FEATURE_GROUP_VERSION=1 \
		--env LIVE_OR_HISTORICAL=live \
		kafka-to-feature-store

run-historical: build
	docker run \
		--network=redpanda_network \
		--env KAFKA_BROKER_ADDRESS=redpanda-0:9092 \
		--env KAFKA_TOPIC_NAME=ohlc_historical \
		--env KAFKA_CONSUMER_GROUP=ohlc_historical_consumer_group_NEW \
		--env FEATURE_GROUP_NAME=ohlc_feature_group \
		--env FEATURE_GROUP_VERSION=1 \
		--env BUFFER_SIZE=150000 \
		--env LIVE_OR_HISTORICAL=historical \
		--env SAVE_EVERY_N_SEC=30 \
		--env CREATE_NEW_CONSUMER_GROUP=true \
		--env HOPSWORKS_PROJECT_NAME=${currencypairml} \
		--env HOPSWORKS_API_KEY=${VrH7IGfUCmeEDhm9.ftaj4jyJIVdStsakGZP22CIxIG5VQFS02bwLZ8eQkv0kujUvBzWKi9w5OXiu6zAS} \
		kafka-to-feature-store

lint:
	poetry run ruff check --fix

format:
	poetry run ruff format .

lint-and-format: lint format