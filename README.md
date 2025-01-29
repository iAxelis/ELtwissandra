debes tener el cassandra obviamente , y crear las base en si y las tablas en cqlsh-- Crear el Keyspace (Base de Datos)
CREATE KEYSPACE IF NOT EXISTS twissandra
WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 1};

-- Seleccionar el Keyspace
USE twissandra;

-- Crear la tabla de usuarios
CREATE TABLE IF NOT EXISTS users (
    id UUID PRIMARY KEY,
    username TEXT,
    email TEXT,
    password TEXT
);

-- Crear la tabla de seguidores (followers)
CREATE TABLE IF NOT EXISTS followers (
    user_id UUID,
    follower_id UUID,
    PRIMARY KEY (user_id, follower_id)
);

-- Crear la tabla de tweets (publicaciones)
CREATE TABLE IF NOT EXISTS tweets (
    tweet_id UUID PRIMARY KEY,
    user_id UUID,
    content TEXT,
    created_at TIMESTAMP
);

-- Crear la tabla de userline (publicaciones de cada usuario)
CREATE TABLE IF NOT EXISTS userline (
    user_id UUID,
    tweet_id UUID,
    content TEXT,
    created_at TIMESTAMP,
    PRIMARY KEY (user_id, tweet_id)
) WITH CLUSTERING ORDER BY (tweet_id DESC);

-- Crear la tabla de friends (relaciones de amigos)
CREATE TABLE IF NOT EXISTS friends (
    user_id UUID,
    friend_id UUID,
    PRIMARY KEY (user_id, friend_id)
);

-- Crear la tabla de timeline (publicaciones de todos los usuarios que siguen)
CREATE TABLE IF NOT EXISTS timeline (
    tweet_id UUID PRIMARY KEY,
    user_id UUID,
    content TEXT,
    created_at TIMESTAMP
);
