
import streamlit as st
import pandas as pd
from datetime import datetime

# Page configuration
st.set_page_config(page_title="Title Registration Tracker", layout="wide")

# Sample society requirements mapping
society_requirements = {
    "AGICOA": ["Agreement", "Conversion", "Portal"],
    "ANGOA": ["Agreement"],
    "DAMA": ["Agreement", "Conversion"],
    "EGEDA": ["Agreement", "Conversion"],
    "GEDIPE": ["Agreement"],
    "GWFF": ["Agreement", "Conversion"],
    "FILMJUS": ["Agreement"],
    "FRF": ["Agreement"],
    "MPAA": ["Agreement"],
    "MPLC": ["Agreement"],
    "PBS": ["Agreement"],
    "PRD": ["Agreement", "Portal"],
    "PROCIREP": ["Agreement"],
    "SACEM": ["Agreement", "Conversion"],
    "SCREENRIGHTS": ["Agreement", "Conversion", "Portal"],
    "SUISSIMAGE": ["Conversion"],
    "SGAE": ["Agreement", "Conversion"],
    "SEKAM": ["Agreement"],
    "UPFAR ARGOA": ["Agreement"],
    "VIDEMA": ["Agreement"],
    "ZAPA": ["Agreement"],
    "CCC (US works)": ["Authorization Form", "Agreement", "Title registration"],
    "CRC (non US works)": ["Authorization Form & Letter of Direction", "Agreement", "Schedule A"],
    "SABAM": [],
    "CCG": [],
    "MPAA (duplicate)": [],
    "PBS (duplicate)": []
}

# Initialize session state
if "titles" not in st.session_state:
    st.session_state.titles = pd.DataFrame(columns=[
        "Title Name", "Client", "Society", "Requirements", "Status", "Pending Date", "Sent Date"
    ])

# Header
st.title("🎬 Title Registration Tracker")

# Upload section
st.sidebar.header("📤 Upload Titles")
uploaded_file = st.sidebar.file_uploader("Upload Excel file", type=["xlsx"])
client_name = st.sidebar.text_input("Client Name")

if uploaded_file and client_name:
    df = pd.read_excel(uploaded_file, engine="openpyxl")
    if "Title Name" in df.columns:
        for _, row in df.iterrows():
            for society, reqs in society_requirements.items():
                st.session_state.titles.loc[len(st.session_state.titles)] = [
                    row["Title Name"], client_name, society, ", ".join(reqs), "Pending",
                    datetime.today().strftime("%Y-%m-%d"), ""
                ]
        st.sidebar.success("Titles uploaded and initialized.")
    else:
        st.sidebar.error("Excel must contain a 'Title Name' column.")

# Filter section
st.sidebar.header("🔍 Filter Titles")
selected_client = st.sidebar.selectbox("Filter by Client", ["All"] + sorted(st.session_state.titles["Client"].unique()))
selected_status = st.sidebar.selectbox("Filter by Status", ["All", "Pending", "Sent"])

# Filter logic
filtered_df = st.session_state.titles.copy()
if selected_client != "All":
    filtered_df = filtered_df[filtered_df["Client"] == selected_client]
if selected_status != "All":
    filtered_df = filtered_df[filtered_df["Status"] == selected_status]

# Display table
st.subheader("📋 Title Overview")
st.dataframe(filtered_df, use_container_width=True)

# Email dispatch simulation
st.subheader("📧 Email Dispatch")
auto_send = st.checkbox("Enable Auto-Send")
recipient_email = st.text_input("Recipient Email", value="nadja@example.com")
if st.button("Simulate Send"):
    for i in st.session_state.titles.index:
        if st.session_state.titles.at[i, "Status"] == "Pending":
            st.session_state.titles.at[i, "Status"] = "Sent"
            st.session_state.titles.at[i, "Sent Date"] = datetime.today().strftime("%Y-%m-%d")
    st.success("All pending titles marked as sent.")

# Analytics placeholder
st.subheader("📊 Analytics & Reporting")
st.info("Analytics and charts will be added in future versions.")

# Admin section
st.subheader("⚙️ Admin & Settings")
if st.checkbox("Show Society Requirements"):
    st.json(society_requirements)
