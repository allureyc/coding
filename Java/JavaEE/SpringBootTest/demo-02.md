```java
package com.ikea.cr.activity.controller;

import com.alibaba.fastjson.JSON;
import com.ikea.cr.activity.ActivityApplication;
import com.ikea.cr.activity.bean.request.ListRestaurantCustomerInfoReq;
import com.ikea.cr.activity.bean.request.SaveCustomerInfoReq;
import lombok.SneakyThrows;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.http.MediaType;
import org.springframework.test.context.ActiveProfiles;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.request.MockMvcRequestBuilders;
import org.springframework.test.web.servlet.result.MockMvcResultMatchers;

import javax.annotation.Resource;

import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;

/**
 * @author Jeff
 * created on 2020/2/18 15:45.
 */
@AutoConfigureMockMvc
@RunWith(SpringRunner.class)
@SpringBootTest(classes = {ActivityApplication.class})
@ActiveProfiles("local")
public class RestaurantControllerTest {

    @Resource
    private MockMvc mockMvc;

    @SneakyThrows
    @Test
    public void saveCustomerInfoTest() {
        mockMvc.perform(
                MockMvcRequestBuilders.post("/restaurant/saveCustomerInfo")
                        .contentType(MediaType.APPLICATION_JSON_VALUE)
                        .content(JSON.toJSONString(
                                SaveCustomerInfoReq.builder()
                                        .name("jeff")
                                        .phone("15837516673")
                                        .peopleCount(2)
                                        .openId("dad2d3dw")
                                        .remark("re")
                                        .unionId("21321")
                                        .area("餐厅")
                                        .storeCode("815")
                                        .build())))
                .andDo(print())
                .andExpect(MockMvcResultMatchers.status().isOk());
    }

    @SneakyThrows
    @Test
    public void queryCustomerInfoTest() {
        mockMvc.perform(
                MockMvcRequestBuilders.get("/restaurant/getCustomerInfo")
                        .param("openId","o2Q9X41nC-idVGa69Gyss75xFNtU"))
                .andDo(print())
                .andExpect(MockMvcResultMatchers.status().isOk());
    }

    @SneakyThrows
    @Test
    public void list() {
        mockMvc.perform(
                MockMvcRequestBuilders.post("/restaurant/list")
                        .contentType(MediaType.APPLICATION_JSON_VALUE)
                        .content(JSON.toJSONString(
                                ListRestaurantCustomerInfoReq.builder()
                                        .page(1)
                                        .size(10)
                                        .area("餐厅")
                                        .storeCode("815")
                                        .build())))
                .andDo(print())
                .andExpect(MockMvcResultMatchers.status().isOk());
    }

}

```

