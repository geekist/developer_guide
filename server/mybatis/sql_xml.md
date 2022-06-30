

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.yuya.core.mapper.CCourseCollectionMapper">
  <resultMap id="BaseResultMap" type="com.yuya.common.entity.console.organ.CCourseCollection">
    <!--
      WARNING - @mbg.generated
    -->
    <id column="id" jdbcType="INTEGER" property="id" />
    <result column="title" jdbcType="VARCHAR" property="title" />
    <result column="cover_url" jdbcType="VARCHAR" property="coverUrl" />
    <result column="description" jdbcType="VARCHAR" property="description" />
    <result column="lecturer" jdbcType="VARCHAR" property="lecturer" />
    <result column="total_num" jdbcType="INTEGER" property="totalNum" />
    <result column="update_num" jdbcType="INTEGER" property="updateNum" />
    <result column="is_top" jdbcType="BIT" property="isTop" />
    <result column="create_time" jdbcType="TIMESTAMP" property="createTime" />
    <result column="update_time" jdbcType="TIMESTAMP" property="updateTime" />
    <result column="type" jdbcType="INTEGER" property="type" />
    <result column="is_deleted" jdbcType="BIT" property="isDeleted" />    
    <result column="content" jdbcType="LONGVARCHAR" property="content" />
  </resultMap>
  
  <sql id="Base_Column_List">
    id, title, cover_url, description, lecturer, total_num, update_num, is_top, create_time, 
    update_time, type, is_deleted,content
  </sql>
  
  <select id="findById" resultMap="BaseResultMap">
  	select <include refid="Base_Column_List"/> from c_course_collection where is_deleted = 0 and id = #{id}
  </select>
  ......
</mapper>
```