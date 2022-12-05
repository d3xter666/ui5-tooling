FROM squidfunk/mkdocs-material:8.5.9

RUN mkdir ./src
RUN chown -R $(id -u):$(id -g) ./src

RUN git clone https://github.com/jimporter/mike.git
RUN git reset --hard e77357960886f9d9bf2e6ecbc39c7ca6991a2179

# RUN pip install mike
RUN pip install -e git+https://github.com/jimporter/mike.git@e77357960886f9d9bf2e6ecbc39c7ca6991a2179#egg=mike --target ./src

# Define env variables for mike
ENV GIT_COMMITTER_NAME="OpenUI5 Bot"
ENV GIT_COMMITTER_EMAIL="openui5@sap.com"
