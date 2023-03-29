# desfio-dynamodb-dio
## Desafio de boas práticas no DynamoDB para o bootcamp Java Developer da DIO

Features Relacionais (SQL) e Não Relacionais (NoSQL) com o DynamoDB.

Assim como no exemplo do desfio criei uma tabela Music utilizando os serviços da Amazon DynamoDB e Amazon CLI para execução em linha de comando.
Em adição ao exemplo eu criei uma nova tabela MyPlaylist com músicas de uma das minhas playlists do Spotify.

### Comandos utilizados:
Criar nova tabela
'''aws dynamodb create-table  --table-name MyPlaylist  --attribute-definitions  AttributeName=Artist,AttributeType=S  AttributeName=SongTitle,AttributeType=S  --key-schema  AttributeName=Artist,KeyType=HASH  AttributeName=SongTitle,KeyType=RANGE  --provisioned-throughput  ReadCapacityUnits=10,WriteCapacityUnits=5'''

Inserir um item
'''aws dynamodb put-item --table-name MyPlaylist --item file://itemmusic2.json'''

Inserir múltiplos itens
'''aws dynamodb batch-write-item --request-items file://batchplaylist.json'''

Criar um index global secundário baeado no título do álbum
'''aws dynamodb update-table --table-name MyPlaylist AttributeName=Artist,AttributeType=S  AttributeName=AlbumTitle,AttributeType=S --global-secondary-index-updates "[{\"Create\":{\"IndexName\": \"ArtistAlbumTitle-index\",\"KeySchema\":[{\"AttributeName\":\"Artist\",\"KeyType\":\"HASH\"}, {\"AttributeName\":\"AlbumTitle\",\"KeyType\":\"RANGE\"}], \"ProvisionedThroughput\": {\"ReadCapacityUnits\": 10, \"WriteCapacityUnits\": 5      },\"Projection\":{\"ProjectionType\":\"ALL\"}}}]"'''

Criar um index global secundário baseado no nome do artista e no título do álbum
'''aws dynamodb update-table --table-name MyPlaylist --attribute-definitions AttributeName=AlbumTitle,AttributeType=S --global-secondary-index-updates "[{\"Create\":{\"IndexName\": \"AlbumTitle-index\",\"KeySchema\":[{\"AttributeName\":\"AlbumTitle\",\"KeyType\":\"HASH\"}], \"ProvisionedThroughput\": {\"ReadCapacityUnits\": 10, \"WriteCapacityUnits\": 5      },\"Projection\":{\"ProjectionType\":\"ALL\"}}}]"'''
