# Deno-6
การบ้านครั้งที่ 6: 2022-09-18
1. Run deno run server.ts --allow-net --alow-read

##  http://10.244.2.108:8000/index.html

FROM denoland/deno:1.25.2
EXPOSE 8000
WORKDIR /app
USER deno
COPY server.ts .
RUN deno cache server.ts
ADD . .
RUN deno cache server.ts
CMD ["run", "--allow-read", "--allow-net", "server.ts"]


## <New version>
<FROM denoland/deno:1.26.0
EXPOSE 8000
WORKDIR /app
USER deno
COPY server.ts .
RUN deno cache server.ts
COPY . .
CMD ["run", "--allow-read", "--allow-net", "server.ts"] />



## <Muliti Statge>

<FROM denoland/deno:1.26.0 AS base
WORKDIR /app
USER deno
FROM base AS requirements
COPY server.ts .
RUN deno cache server.ts
COPY . .
FROM requirements AS build  
WORKDIR /app
FROM denoland/deno:alpine AS release  
WORKDIR /app
USER deno
COPY --from=requirements /app/server.ts ./
RUN deno cache server.ts
COPY . .
EXPOSE 8000
CMD ["run", "--allow-read", "--allow-net", "server.ts"] >





## < Multi > ##
<FROM denoland/deno:1.26.0 AS base
WORKDIR /app
USER deno
FROM base AS requirements
COPY server.ts .
RUN deno cache server.ts
COPY . .
FROM requirements AS build  
WORKDIR /app
FROM denoland/deno:alpine-1.10.3 AS release  
WORKDIR /app
COPY --from=requirements /app/server.ts ./
EXPOSE 8000
CMD ["run", "--allow-read", "--allow-net", "server.ts"]  >
