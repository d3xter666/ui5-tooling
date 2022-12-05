FROM squidfunk/mkdocs-material:8.5.9

RUN mkdir ./src
RUN chown -R $(id -u):$(id -g) ./src

RUN mkdir mike && \
	cd mike && \
	git init && \
	git remote add origin https://github.com/jimporter/mike.git && \
	git fetch --all && \
	git checkout e77357960886f9d9bf2e6ecbc39c7ca6991a2179
# git clone https://github.com/jimporter/mike.git && git checkout e77357960886f9d9bf2e6ecbc39c7ca6991a2179

# RUN pip install mike
# RUN pip install -e git+https://github.com/jimporter/mike.git@e77357960886f9d9bf2e6ecbc39c7ca6991a2179#egg=mike --target ./src
RUN pip install -e ./mike

# Define env variables for mike
ENV GIT_COMMITTER_NAME="OpenUI5 Bot"
ENV GIT_COMMITTER_EMAIL="openui5@sap.com"
