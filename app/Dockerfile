FROM renovate/poetry as base

COPY --chown=onnauser:onnagroup ./ .

EXPOSE 8080
CMD ["make", "run-http"]

FROM base as production
RUN poetry install --no-dev
USER root
RUN apt remove -y --purge binutils libc6-dev gcc --allow-remove-essential
USER onnauser

FROM base as test
RUN poetry install
USER root
RUN apt remove -y --purge binutils libc6-dev gcc --allow-remove-essential
USER onnauser
