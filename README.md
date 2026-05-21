# test1
import streamlit as st
import pandas as pd

st.set_page_config(page_title="스케줄표 앱")

st.title("📅 나의 스케줄표")

st.write("오늘의 일정을 추가해보세요!")

# 입력 영역
schedule = st.text_input("일정 입력")
time = st.time_input("시간 선택")

# 세션 상태 만들기
if "schedule_list" not in st.session_state:
    st.session_state.schedule_list = []

# 추가 버튼
if st.button("일정 추가"):
    st.session_state.schedule_list.append({
        "시간": str(time),
        "일정": schedule
    })

    st.success("일정이 추가되었습니다!")

# 데이터프레임 생성
if st.session_state.schedule_list:

    df = pd.DataFrame(st.session_state.schedule_list)

    st.subheader("📌 현재 일정표")

    st.table(df)

    # CSV 다운로드
    csv = df.to_csv(index=False).encode("utf-8-sig")

    st.download_button(
        label="CSV 다운로드",
        data=csv,
        file_name="schedule.csv",
        mime="text/csv"
    )

else:
    st.info("아직 일정이 없습니다.")
