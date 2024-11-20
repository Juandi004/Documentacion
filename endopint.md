# Enpoint para saber si un topic está en modo "enabled"

## Código

### Service
```
  async getTopicStatus(topicId: string) {
    try {
      const topic = await this.prisma.topic.findUnique({ where: { topicId } });
      if (!topic) {
        throw new HttpException('Topic not found', HttpStatus.NOT_FOUND);
      }
      return topic.enabled;
    } catch (error) {
      if (error.status === HttpStatus.NOT_FOUND) {
        throw error;
      }
      throw new HttpException(
        'Error fetching topic',
        HttpStatus.INTERNAL_SERVER_ERROR,
      );
    }
  }
```

### Controller
```
  @Get(':topicId/is-enabled')
  @Auth(ValidRoles.admin, ValidRoles.teacher)
  @ApiOperation({ summary: 'Get if a topic is enabled' })
  async getStatus(@Param('topicId') topicId: string) {
      return await this.topicsService.getTopicStatus(topicId);
  }
```

### Funcionalidad:
#### Este endpoint nos permite saber si un topic se encuentra activo mediante la propiedad 'enabled' en la interface de Topic


#### Realizado por: Juan Gutierrez