```java
package com.ikea.cr.activity.controller;

import com.alibaba.fastjson.JSON;
import com.ikea.cr.activity.ActivityApplication;
import com.ikea.cr.activity.bean.request.ExportRaiseHandReq;
import com.ikea.cr.activity.bean.request.RaiseHandParamReq;
import lombok.SneakyThrows;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.http.MediaType;
import org.springframework.test.context.ActiveProfiles;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.request.MockMvcRequestBuilders;
import org.springframework.test.web.servlet.result.MockMvcResultMatchers;

import javax.annotation.Resource;

import java.util.ArrayList;

import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;

/**
 * @Author guohang
 * @Description
 * @Date 2020/7/10 14:21
 */
@AutoConfigureMockMvc
@RunWith(SpringRunner.class)
@SpringBootTest(classes = {ActivityApplication.class})
@ActiveProfiles("local")
public class RaiseHandControllerTest {

    @Resource
    private MockMvc mockMvc;

    @Test
    public void getStatusAndAddress() throws Exception {
        mockMvc.perform(
                MockMvcRequestBuilders.get("/raiseHandMember/getStatusAndAddress/" + "618283"))
                .andDo(print())
                .andExpect(MockMvcResultMatchers.status().isOk()).andReturn();
    }

    @SneakyThrows
    @Test
    public void getRaiseHandMember() {
        mockMvc.perform(
                MockMvcRequestBuilders.post("/raiseHandMember/getRaiseHandMember")
                        .contentType(MediaType.APPLICATION_JSON_VALUE)
                        .content(JSON.toJSONString(
                                RaiseHandParamReq.builder()
                                        .pageNum(1)
                                        .pageSize(5)
                                        .preferStore("644")
                                        .customerName("张三")
                                        .phone("13188057705")
                                        .memberCard("222222")
                                        .customerId(618283)
                                        .wechatUnionid("ombkt1AnHSurLaGV85PLRJziH6LI")
                                        .build())))
                .andDo(print())
                .andExpect(MockMvcResultMatchers.status().isOk());
    }

    @SneakyThrows
    @Test
    public void exportRaiseHand() {
        ArrayList<Integer> list = new ArrayList<>();
        list.add(618282);
        list.add(618283);
        mockMvc.perform(
                MockMvcRequestBuilders.post("/raiseHandMember/exportRaiseHand")
                        .contentType(MediaType.APPLICATION_JSON_VALUE)
                        .content(JSON.toJSONString(
                                ExportRaiseHandReq.builder()
                                        .cid(list)
                                        .build())))
                .andDo(print())
                .andExpect(MockMvcResultMatchers.status().isOk());
    }

}




```

