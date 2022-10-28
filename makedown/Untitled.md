## 切换年月对应数据所需要的type

| type                   | 对应字段       |
| ---------------------- | -------------- |
| overload_num           | 重过载配变数量 |
| unbalance_num          | 三相不平衡数量 |
| voltage_qualified_num  | 电压合格数量   |
| ele_off_time           | 停电时间       |
| ele_charge_num         | 电费回收数量   |
| ele_loss_num           | 线损数量       |
| ele_loss_exception_num | 线损异常数量   |
| nine_hot_num           | 95598热线数量  |
| one_hot_num            | 12398热线数量  |
| complain_num           | 投诉数量       |

```java
<sql id="Base_Column_List_HjUserInfo">
        aid,aname,
        uname,uage,usex,
        upicture,ulocal,unation,
        up_height,up_weight,up_blood,
        up_health,up_health_des,up_medical_his,
        up_medical_his_des,uid_code,uid_adderss,
        uaddress,uphone,umarital,
        upolotical,ureproductive,uhousehold
    </sql>

    <!--HjUserInfoVO一对多映射-->
    <resultMap id="HjUserInfoMap" type="com.bda.huijun.hr.xjh.entity.vo.HjUserInfoVO">
        <result column="ID" property="id" />
        <result column="AID" property="aid" />
        <result column="ANAME" property="aname" />
        <result column="UNAME" property="uname" />
        <result column="UAGE" property="uage" />
        <result column="USEX" property="usex" />
        <result column="UPICTURE" property="upicture" />
        <result column="ULOCAL" property="ulocal" />
        <result column="UNATION" property="unation" />
        <result column="UP_HEIGHT" property="upHeight" />
        <result column="UP_WEIGHT" property="upWeight" />
        <result column="UP_BLOOD" property="upBlood" />
        <result column="UP_HEALTH" property="upHealth" />
        <result column="UP_HEALTH_DES" property="upHealthDes" />
        <result column="UP_MEDICAL_HIS" property="upMedicalHis" />
        <result column="UP_MEDICAL_HIS_DES" property="upMedicalHisDes" />
        <result column="UID_CODE" property="uidCode" />
        <result column="UID_ADDERSS" property="uidAdderss" />
        <result column="UADDRESS" property="uaddress" />
        <result column="UPHONE" property="uphone" />
        <result column="UMARITAL" property="umarital" />
        <result column="UPOLOTICAL" property="upolotical" />
        <result column="UREPRODUCTIVE" property="ureproductive" />
        <result column="UHOUSEHOLD" property="uhousehold" />
        <collection  property="contacts" ofType="com.bda.huijun.hr.xjh.entity.dto.HjUserContactDTO">
            <result column="UID" property="uid" />
            <result column="CNAME" property="cname" />
            <result column="CPHONE" property="cphone" />
            <result column="CADDRESS" property="caddress" />
        </collection>
        <collection property="educations" ofType="com.bda.huijun.hr.xjh.entity.dto.HjUserEducationDTO">
            <result column="UID" property="uid" />
            <result column="ENAME" property="ename" />
            <result column="ETYPE" property="etype" />
            <result column="EMAHOR" property="emahor" />
            <result column="ESTARTTIME" property="estarttime" />
            <result column="EENDTIME" property="eendtime" />
        </collection>
    </resultMap>
    <!--HjUserContactDTO映射-->
    <resultMap id="hjUserContactDTOMap" type="com.bda.huijun.hr.xjh.entity.dto.HjUserContactDTO">
        <result column="UID" property="uid" />
        <result column="CNAME" property="cname" />
        <result column="CPHONE" property="cphone" />
        <result column="CADDRESS" property="caddress" />
        <result column="EXT1" property="ext1" />
        <result column="EXT2" property="ext2" />
    </resultMap>
    <!--HjUserEducationDTO映射-->
    <resultMap id="hjUserEducationDTOMap" type="com.bda.huijun.hr.xjh.entity.dto.HjUserEducationDTO">
        <result column="UID" property="uid" />
        <result column="ENAME" property="ename" />
        <result column="ETYPE" property="etype" />
        <result column="EMAHOR" property="emahor" />
        <result column="ESTARTTIME" property="estarttime" />
        <result column="EENDTIME" property="eendtime" />
        <result column="EXT1" property="ext1" />
        <result column="EXT2" property="ext2" />
    </resultMap>

    <select id="getHjUserInfoById" resultMap="HjUserInfoMap">
        SELECT <include refid="Base_Column_List_HjUserInfo" />,
               huc.UID, huc.CNAME, huc.CPHONE, huc.CADDRESS,
               hue.UID, hue.ENAME, hue.ETYPE, hue.EMAHOR, hue.ESTARTTIME, hue.EENDTIME
        FROM hj_user_info hui
        LEFT JOIN hj_user_contact huc ON hui.aid = huc.uid
        LEFT JOIN hj_user_education hue ON hui.aid = hue.uid
        WHERE hui.aid = #{id}
    </select>

    <select id="getHjUserInfoByObject" resultMap="HjUserInfoMap" >
        SELECT hui.id, <include refid="Base_Column_List_HjUserInfo" />,
            huc.UID, huc.CNAME, huc.CPHONE, huc.CADDRESS,
            hue.UID, hue.ENAME, hue.ETYPE, hue.EMAHOR, hue.ESTARTTIME, hue.EENDTIME
        FROM hj_user_info hui
        LEFT JOIN hj_user_contact huc ON hui.aid = huc.uid
        LEFT JOIN hj_user_education hue ON hui.aid = hue.uid
        <where>
            <if test="#{dto.aid} != '0'">
                AND hui.aid = '${dto.aid}'
            </if>
            <if test="null != #{dto.aname}">
                AND hui.aname like '%${dto.aname}%'
            </if>
            <if test="null != #{dto.uname}">
                AND hui.uname like '%${dto.uname}%'
            </if>
<!--            <if test="null != #{dto.uage}">-->
<!--                AND hui.uage = '${dto.uage}'-->
<!--            </if>-->
<!--            <if test="null != #{dto.usex}">-->
<!--                AND hui.usex = '${dto.usex}'-->
<!--            </if>-->
<!--            <if test="null != #{dto.ulocal}">-->
<!--                AND hui.ulocal like '%${dto.ulocal}%'-->
<!--            </if>-->
<!--            <if test="null != #{dto.unation}">-->
<!--                AND hui.unation = '${dto.unation}'-->
<!--            </if>-->
<!--            <if test="null != #{dto.upBlood}">-->
<!--                AND hui.up_blood = '${dto.upBlood}'-->
<!--            </if>-->
<!--            <if test="null != #{dto.upHealth}">-->
<!--                AND hui.up_health = '${dto.upHealth}'-->
<!--            </if>-->
<!--            <if test="null != #{dto.upMedicalHis}">-->
<!--                AND hui.up_medical_his = '${dto.upMedicalHis}'-->
<!--            </if>-->
<!--            <if test="null != #{dto.uidCode}">-->
<!--                AND hui.uid_code = '${dto.uidCode}'-->
<!--            </if>-->
<!--            <if test="null != #{dto.uphone}">-->
<!--                AND hui.uphone = '${dto.uphone}'-->
<!--            </if>-->
<!--            <if test="null != #{dto.upolotical}">-->
<!--                AND hui.upolotical = '${dto.upolotical}'-->
<!--            </if>-->
<!--            <if test="null != #{dto.uhousehold}">-->
<!--                AND hui.uhousehold = '${dto.uhousehold}'-->
<!--            </if>-->
        </where>
    </select>
```

